`hot-ajax-paging`
Element to control an iron-ajax element

This element adds paging capabilities to iron-ajax. It also provides _all_ the information a pager element might possibly need to draw itself.
To use it, simply wrap an iron-ajax widget with it:

    <hot-ajax-paging per-page="10" current-page="{{currentPage}}" pager-data="{{pagerData}}">
      <iron-ajax auto url="/stores/polymer" handle-as="json" last-response="{{data}}"></iron-ajax>
    </hot-ajax-paging>

Interaction with the iron-ajax widget happens as follows:

* **Outgoing:** Whenever current-page changes, a `range` header is added to the `iron-ajax` widget, in the format `items=0-9` which will instruct the server only to return records 0 to 9.

* **Incoming:** Whenever the server responds with results, a header in the format `Content-Range items 0-24/66` is expected, where 66 is the total number of records. This information is used to effectively make the pager functional and properly sized. Before the first AJAX call returns, the pager won't even display (since it doesn't know how many pages it should show). Once the first AJAX call returns, the pager will display with the right information.

## pagerData

The (notifying) pagerData variable is updated every time anything about the pager changes. The `pagerData` variable will have the following fields:

* `items` - An array with the list of items. Each one is an object like this: `{page: 1, current: true}`. There will only be one item with its `current` property set to true
* `left` - The page linked by the item in the page on the far left
* `right` - The page linked by the item in the page on the far left
* `moreToLeft` - set to `true` if the most left item in the pager is not page 1
* `moreToRight` - set to `true` if the most right item in the pager is not the last page available
* `jumpNext` - the page number for the next "page"
* `jumpPrev` - the page number for the previous "page".
* `from` and `to` - the range of records shown. For example if there are 15 records and the current page is 1, `from` and `to` will be `0` and `1` respectively. For page 2, they will be `2` and `3`, and so on.
* `perPage` - How many records will are shown in each page
* `currentPage` - The current page
* `maxShownPages` - How many "boxes" the pager will display
* `totalPages` - The total number of pages
* `totalRecords` - The total number of records


## Setting properties

The pager needs to be set up.

* `maxShownPages` - The maximum number of pages the pager will ever show. Changing this value will implying firing up an AJAX request.
* `from` and `to` - The range of the records shown. Changing either one of them will fire up a new AJAX request. Also, changing `from` will possibly change `currentPage`
* `perPage` - How many records are shown in each page. Changing this value will _not_ fire up a new AJAX request. However, `currentPage` will likely change without firing up an AJAX request (since changing perPage will likely mean that you are looking at a different page, but the data is already loaded). Note that only `from` is taken into consideration. So, if you make perPage smaller, you will end up looking at a different page, showing too many records.
* `currentPage` - The current page. Changing this value will fire up a new AJAX request.

## Custom target

## Using a custom target ID

It's possible to specify the target ID directly and have the target input field further in the DOM, rather than being `<hot-ajax-paging>`'s first child:

    <hot-ajax-paging target-id="aj" per-page="10" current-page="{{currentPage}}" pager-data="{{pagerData}}">
      <div></div>
      <iron-ajax id="aj" auto url="/stores/polymer" handle-as="json" last-response="{{data}}"></iron-ajax>
    </hot-ajax-paging>

