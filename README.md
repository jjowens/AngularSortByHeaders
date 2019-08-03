# AngularSortByHeaders

## Overview
A wee script to sort books by header text or by data-sortby attribute.

## Details
The data is a JSON file manually created. It contains a small selection of books. Each book record has a title, author name and genre.

When you click on the table header text e.g. title, author, it will sort the data based on that header text. Clicking on the same header will re-order the data in reverse. For example, clicking on Genre will sort books from Z to A.

## Sort By Headers function

Books are retrieved from a JSON and is loaded into a table.

```html
<tr ng-repeat="book in books | orderBy:orderByLabel:reverseSort">
  <td>{{ book.Title }}</td>
  <td>{{ book.Author.FirstName }} {{book.Author.LastName}}</td>
  <td>{{ book.Genre }}</td>
  <td>&nbsp;</td>
</tr>
```     


### Initialise Order By and Sort Order

Initialise the order as Author.LastName and reverseSort as false. Setting it as false tells angular to sort the data from A to Z. When data is loaded is sort by Author's LastName and from A to Z.

 ```javascript
 $scope.orderByLabel = "Author.LastName";
 
 $scope.reverseSort = false;
```

Inside the table row, it will books by the orderByLabel and reverseSort

```html
<tr ng-repeat="book in books | orderBy:orderByLabel:reverseSort">
```


### Create a Sort Link

When you add Header text, wrap it with an anchor link. The anchor link must have a ng-click attribute pointing to sortByHeader function.

```html
<th><a href="#" ng-click="sortByHeader($event.currentTarget)">Title</a></th>
```

Add the following function 'sortByHeader'. Comments 

```javascript
$scope.sortByHeader = function sortByHeader(element) {
            
            // GET HEADER TEXT
            var newOrderBy = element.text;

            // SET SORT ORDER
            var reverseSort = false;

            // CHECK IF HEADER HAS A DATA-ORDERBY ATTRIBUTE
            if (element.dataset !== undefined) {
                if (element.dataset.orderby !== undefined) {
                    newOrderBy = element.dataset.orderby;
                }
            }

            // CHECK IF NEW SORT CATEGORY MATCHES UP TO THE CURRENT ONE. IT MEANS IT HAS BEEN CLICKED ON MORE THAN ONCE
            // IF SO, REVERSE THE SORT ORDER. IF NOT, SET SORT ORDER FROM A TO Z
            if (newOrderBy === $scope.orderByLabel) {
                reverseSort = !$scope.reverseSort;
            } else {
                reverseSort = false;
            }

            // UPDATE ORDERBY AND SORT ORDER
            $scope.orderByLabel = newOrderBy;
            $scope.reverseSort = reverseSort;
        }
```

### Get Header Text/data-orderby
When you click on the link, it will fire the sortByHeader function. It will pick up the header text from the element object and use that text to sort the data by. The element object came from the $event.currentTarget inside the ng-click attribute. 

This $event.currentTarget has a few useful properties such as text, innerHTML where you can parse or manipulate those properties.

Once it gets the header text, it will also check if the header text/element has a data-orderby attribute. This will be used to override the header text. The data-orderby attribute is useful if you want to have a different header text but don't want to use the header text for sorting. 

```
// CHECK IF HEADER HAS A DATA-ORDERBY ATTRIBUTE
if (element.dataset !== undefined) {
    if (element.dataset.orderby !== undefined) {
        newOrderBy = element.dataset.orderby;
    }
}
```

Example, you can click on Imaginative People, the data-orderby tells Angular to sort the records by 'Author.LastName', not by 'Imaginative People' (a label which does not exists in the JSON file).

```html
<th><a href="#" ng-click="sortByHeader($event.currentTarget)" data-orderby="Author.LastName">Imaginative People</a></th>
```

### Reverse Order

There is a logic check where it will reverse the order of books if you already clicked on the header text. 

It picks up the header text

```javascript
// GET HEADER TEXT
var newOrderBy = element.text;
```

Then it will check the newOrderBy against the current orderByLabel. If it matches up, it will reverse the order. Otherwise, it will sort the data from A to Z from a new category.

```javascript
if (newOrderBy === $scope.orderByLabel) {
    reverseSort = !$scope.reverseSort;
} else {
    reverseSort = false;
}
```

And that's it. Feel free to borrow it. 
