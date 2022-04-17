---
title: angularjs实现分页
date: 2021-10-31 13:30:49
permalink: /pages/8299d4/
categories:
  - Angular
tags:
  - 
---
## angularjs实现分页

#### js
```javascript
var fail_log_app = angular.module("FailLogApp", []);
fail_log_app.controller("FailLogController", function ($scope, $document) {
  $scope.page_range = page_range; // page_range: [1,2,3,4,5,6,7,8, ...]
  $scope.cur_page = 1;
  /**
   * Go to page
   */
  $scope.go_to_page = function(page){
    if (page<=0 || page>$scope.page_range.length){
      return;
    }
    //...
    //...
  }
  /**
   * Generate pagination
   */
  $scope.pagination = function() {
    let c = $scope.cur_page;
    let arr = [1, c-2, c-1, c, c+1, c+2, $scope.page_range.length]
    // filter the invalid page
    arr = arr.filter(item => {
      return item>0 && item<=$scope.page_range.length
    })
    // remove duplicate page
    arr = [...new Set(arr)]
    // insert '...' in left pagination
    if(arr.length>1 && arr[0]+1 !== arr[1]){
        arr.splice(1,0,-1)
    }
    // insert '...' in right pagination
    if (arr.length>1 && arr[arr.length-1]-1 !== arr[arr.length-2]){
        arr.splice(arr.length-1,0,-2)
    }
    return arr;
  }

  $document.bind("keydown", function(event){
    if (event.keyCode === 37) {
      // Left
      if (event.ctrlKey) {
        $scope.go_to_page($scope.cur_page - 100)
      }else {
        $scope.go_to_page($scope.cur_page - 1)
      }

    }else if (event.keyCode === 39) {
      // Right
      if (event.ctrlKey) {
        $scope.go_to_page($scope.cur_page + 100)
      }else {
        $scope.go_to_page($scope.cur_page + 1)
      }
    }

  })

});

```

#### html
```html
<body ng-app="FailLogApp">
    <div class="top_bar_div">
        <p style="text-align: center; font-size: 50px;">Fail Cycle Log Browser</p>
    </div>

    <div ng-controller="FailLogController">
      <!-- Page picker -->
      <div class="d-flex" style="position:absolute; left: 0.5%; top: 60px;">
      {% verbatim %}
      <nav class="mr-3">
        <ul class="pagination pagination-sm">
          <li class="page-item">
            <a
              class="page-link"
              ng-class="cur_page<=1?'disable':'text-primary'"
              ng-click="go_to_page(cur_page-1)"
              ng-style="{cursor:cur_page<=1?'default':'pointer'}"
            >
              <span aria-hidden="true">&laquo;</span>
            </a>
          </li>
          <li ng-class="p===cur_page?'active':''" class="page-item ml-1 mr-1" ng-repeat="p in pagination() track by $index" >
              <a ng-if="p>-1" class="page-link" style="cursor: pointer" ng-click="go_to_page(p)" >{{ p }}</a>
              <a ng-if="p===-1" class="page-link text-secondary" style="cursor: pointer" ng-click="go_to_page(cur_page-5)" >...</a>
              <a ng-if="p===-2" class="page-link text-secondary" style="cursor: pointer" ng-click="go_to_page(cur_page+5)" >...</a>
          </li>
          <li class="page-item">
            <a
              class="page-link"
              ng-class="cur_page>=page_range.length?'disable':'text-primary'"
              ng-click="go_to_page(cur_page+1)"
              ng-style="{cursor:cur_page>=page_range.length?'default':'pointer'}"
            >
              <span aria-hidden="true">&raquo;</span>
            </a>
          </li>
        </ul>
      </nav>
      {% endverbatim %}
      <div style="line-height: 30px;">Total {{ fail_id_num }} cycles</div>
      </div>
    </div>
</body>
```