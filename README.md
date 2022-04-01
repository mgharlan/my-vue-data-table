# my-vue-data-table

A reusable datatable component for Vue.js

## Project Setup

```sh
git clone https://github.com/mgharlan/my-vue-data-table.git
cd my-vue-data-table
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile for Production

```sh
npm run build
```

### Requirements and Report

The VDataTable component takes three parameters: columns, rows, and perPage. The columns and rows specify the data and respective attributes for the columns and rows. The perPage parameter is an optional parameter with a default value of ten.

The page selector on the bottom of the table was beginning to look cramped and could cause issues with a large number of pages. I decided to cap the number of pages visible in the footer to ten. Using the navigation arrows you can access the next ten pages. For example, on the table in the "Testing Larger Result Set" section if you click on "10" then click on the right arrow it will roll over to the next ten available pages to select from. This also works in the other direction.

For the sorting functionality, click on the column you want to sort to toggle between ascending and descending order. I decided to implement a simple string sort on the formatted values. This causes numerical fields such as 'Sale' to show in an unexpected order. Another way to do it would be to sort on the raw values and check the type beforehand to sort accordingly. This would allow for sorting dates and other more specific types with more control. To keep generic and consistent functionality, I went with string sorting. I also considered adding a return to original ordering option but decided that toggling between ascending and descending order was acceptable for this assignment. Given the additional requirements, adding in a return to normal ordering on a third click of the column header could be easily implemented by keeping track of the original ordering.
