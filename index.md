---
layout: minimal
title: Search
nav_order: 2 
---

<link rel="stylesheet" href="./assets/css/widget.css">
<link rel="stylesheet" href="./assets/css/search-tool.css">

<!-- https://jekyllrb.com/tutorials/csv-to-table/ -->
<!-- https://github.com/christian-fei/Simple-Jekyll-Search -->

<div class="search-filter-container">
  <div class="search-container" id="search-container">
    <div class="search-container-left">
      <div class="mobile-search-container">
        <label>
          <input class="tool-search-input" type="text" id="search-inputt" placeholder="Search...">
          <button onclick="toggleMobileFilter();" class="filter-button" type="button">🝖</button>
        </label>
        <div class="filters-container-mobile">
          <fieldset>
            <legend style="font-weight: bold; padding: 0em 0.5em">Filters</legend>
            <details class="filter-dropdown" id="yearsFiltersMobile">
              <summary style="border-bottom: none; margin-bottom: 0; padding-bottom: 0.25em;">Year</summary>
            </details>
            <details class="filter-dropdown" id="seriesFiltersMobile">
              <summary style="border-bottom: none; margin-bottom: 0; padding-bottom: 0.25em;">Series</summary>
            </details>
            <details class="filter-dropdown" id="typeFiltersMobile">
              <summary style="border-bottom: none; margin-bottom: 0; padding-bottom: 0.25em;">Type</summary>
            </details>
            <details class="filter-dropdown" id="topicsFiltersMobile">
              <summary style="border-bottom: none; margin-bottom: 0; padding-bottom: 0.25em;">Topics</summary>
            </details>
            <details class="filter-dropdown" id="softwareFiltersMobile">
              <summary style="border-bottom: none; margin-bottom: 0; padding-bottom: 0.25em;">Software</summary>
            </details>
            <!-- <details class="filter-dropdown" id="certificateFiltersMobile"> -->
              <!-- <summary style="border-bottom: none; margin-bottom: 0; padding-bottom: 0.25em;">Certificate</summary> -->
            <!-- </details>-->
          </fieldset>
          <div class="sidebar-footer-spacer"></div>
        </div>
      </div>
      <div class="results-container" id="results-container">
        <!-- This is where results are automatically filled -->
      </div>
    </div>
    <!-- Filters -->
    <div class="filters-container">
      <fieldset>
        <legend style="font-weight: bold; padding: 0em 0.5em">Filters</legend>
        <details open class="filter-dropdown" id="yearsFilters">
          <summary style="border-bottom: none; margin-bottom: 0; padding-bottom: 0.25em;">Year</summary>
        </details>
        <details open class="filter-dropdown" id="seriesFilters">
          <summary style="border-bottom: none; margin-bottom: 0; padding-bottom: 0.25em;">Series</summary>
        </details>
        <details open class="filter-dropdown" id="typeFilters">
          <summary style="border-bottom: none; margin-bottom: 0; padding-bottom: 0.25em;">Type</summary>
        </details>
        <details open class="filter-dropdown" id="topicsFilters">
          <summary style="border-bottom: none; margin-bottom: 0; padding-bottom: 0.25em;">Topics</summary>
        </details>
        <details open class="filter-dropdown" id="softwareFilters">
          <summary style="border-bottom: none; margin-bottom: 0; padding-bottom: 0.25em;">Software</summary>
        </details>
        <!-- <details class="filter-dropdown" id="certificateFilters"> -->
              <!-- <summary style="border-bottom: none; margin-bottom: 0; padding-bottom: 0.25em;">Certificate</summary> -->
           <!-- </details>*/ -->
      </fieldset>
      <div class="sidebar-footer-spacer"></div>
    </div>
  </div>
</div>

<!-- Script pointing to search-script.js -->
<script src="assets/javascript/jquery.js"></script>
<script src="assets/javascript/search-script.js" type="text/javascript"></script>

<script>
function getProperty(title, prop) {
  try {
    return json[title][prop]; 
  } catch(e) {
    console.log("json not initialized");
  }
}

var title = "";
var json = "";
var search = "";
$.getJSON('data.json', function(obj) {
  json = obj;
  $.getJSON('search.json', function(objj) {
    search = objj;

    var sjs = SimpleJekyllSearch({
      searchInput: document.getElementById('search-inputt'),
      resultsContainer: document.getElementById('results-container'),
      json: search,
      noResultsText: 'No result found!',
      limit: 12,
      fuzzy: false,
      searchResultTemplate: '<!--{title}, {url}-->
      <div class="result-tile">
        <a target="_parent" href="{url}" class="result-link">
        <object data="{image}" type="image/png" height="auto" width="100%" class="result-image" alt="workshop image for {title}">
          <object data="assets/img/{series_image}_image.png" type="image/png" height="auto" width="100%" class="result-image" alt="{series_image} logo for workshop with no specified image.">
            <img src="assets/img/unknownImageLocation.png" height="100%" class="result-image" alt="Sherman Center logo for workshop with no specified image.">
          </object>
        </object>
        <p class="result-title">{title}</p>
        </a>
        <p class="result-details"> {series} - {year} {topics} {software} </p>
      </div>
      ',
      templateMiddleware: function(prop, value, template) {
        if(prop === 'title') {
          title = value;
        }

        if (prop === 'topics') {
          var strr = "";
          function createTopics(topic) { strr = strr.concat(topic, ", ");  }
          if(value == "N/A") {
            return strr;
          } else {
            strr = " - ";
          }
          value = value.split("; ");
          value.forEach(createTopics);
          return strr.slice(0, -2);
        }

        if (prop === 'software') {
          var strr = "";
          function createSoftware(software) { strr = strr.concat(software, ", ");  }
          if(value == "N/A") {
            return strr;
          } else {
            strr = " - ";
          }
          value = value.split("; ");
          value.forEach(createSoftware);
          return strr.slice(0, -2);
        }

        if (prop === 'year') {
          var strr = "";
          function createYear(year) { strr = strr.concat(year, ", ");  }
          value = value.split(";");
          value.forEach(createYear);
          return strr.slice(0, -2);
        }

        if (prop === 'type') {
          var strr = "";
          function createType(type) { strr = strr.concat(type, ", ");  }
          if(value == "N/A") {
            return strr;
          } else {
            strr = " - ";
          }
          value = value.split("; ");
          value.forEach(createType);
          return strr.slice(0, -2);
        }

        /*if (prop === 'certificate') {
          var strr = "";
          function createCertificate(certificate) { strr = strr.concat(certificate, ", ");  }
          if(value == "N/A") {
            return strr;
          } else {
            strr = " - ";
          }
          value = value.split("; ");
          value.forEach(createCertificate);
          return strr.slice(0, -2);
        }*/

        if (prop === 'url' || prop === 'year' || prop === 'series' || prop === 'image') {
          return getProperty(title, prop);
        }

        if (prop === "series_image") {
          return getProperty(title, "series").toLowerCase();
        }
      }
    })
  });
})




</script>
