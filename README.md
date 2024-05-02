# Actor - Redfin Scraper

## Redfin scraper

Since Redfin doesn't provide an API, this actor should help you to retrieve data from it.

The Redfin data scraper supports the following features:

-   Scrape property details - You can scrape attributes like property images, price, features, neighborhood, nearby schools, walkscore, and many more. You can find details below.

-   Scrape sold properties - You can scrape sold properties through a search list.

-   Scrape for sale properties - If you are looking for a property that is for sale, you can directly target them.

-   Scrape rental properties - Rental properties can be directly targeted.

-   Scrape by keyword - You can use location-wise keywords to search specific search lists. Also, you can directly point out rental, for sale, or sold properties on this feature.

-   Scrape properties by filter - Auto detection of URLs helps you to directly copy/paste the URLs into the scraper to apply any filtering you like.

#### Redfin specific

Don't worry when you get a little bit different properties than you saw on the browser page. Redfin is ordering properties differently for each user.

## Bugs, fixes, updates, and changelog

This scraper is under active development. If you have any feature requests you can create an issue from [here](https://github.com/epctex/redfin-scraper/issues).


## Input Parameters

The input of this scraper should be JSON containing the list of pages on Redfin that should be visited. Possible fields are:

- `search`: (Optional) (String) Keyword that can be searched in the Redfin search engine. When it is present, `searchMode` must be used as well.

- `searchMode`: (Optional) (String) Mode of the actor. It gets the keyword from `search` parameter and initiates the search according to the mode. Can be `SALE`, `RENT` or `SOLD`. When present, `search` must be provided as well.

- `startUrls`: (Optional) (Array) List of Redfin URLs. You should only provide property detail or search URLs.

- `endPage`: (Optional) (Number) Final number of page that you want to scrape. The default is `Infinite`. This applies to all `search` requests and `startUrls` individually.

- `maxItems`: (Optional) (Number) You can limit scraped items. This should be useful when you search through the big lists or search results.

- `includeExtras`: (Optional) (Boolean) This option enables the actor to retrieve extra information per each property. Please note that this will increase the runtime of the actor.

- `proxy`: (Required) (Proxy Object) Proxy configuration.

- `customMapFunction`: (Optional) (String) Function that takes each of the objects as argument and returns data that will be mapped by the function itself.

This solution requires the use of **Proxy servers**, either your own proxy servers or you can use [Apify Proxy](https://www.apify.com/docs/proxy).

### Tip

When you want to have filtering over a search URL; go to Redfin, create filters over the search list, and copy and paste the link as one of the **startUrl**.

If you would like to scrape only the first page of a search list or search list, then put the link for the page and have the `endPage` as 1.

With the last approach that is explained above you can also fetch any interval of pages. If you provide the 5th page of a search list and define the `endPage` parameter as 6 then you'll have the 5th and 6th pages only.

### Compute Unit Consumption

The actor is optimized to run blazing fast and scrape many properties as possible. Therefore, it forefronts all property detail requests. If the actor doesn't block very often it'll scrape 100 properties in 2.5 minutes with ~0.06-0.09 compute units.

### Redfin Scraper Input example

```json
{
  "search": "Los Angeles",
  "searchMode": "SALE",
  "startUrls": [
    "https://www.redfin.com/CA/Encino/Legado-Encino/apartment/177407600",
    "https://www.redfin.com/CA/Los-Angeles/1425-W-12th-St-90015/unit-250/home/6932257",
    "https://www.redfin.com/CA/Encino/17846-Palora-St-91316/home/4287600",
    "https://www.redfin.com/CA/West-Hills/7909-Valley-Flores-Dr-91304/home/3059994",
    "https://www.redfin.com/CA/Encino/Legado-Encino/apartment/177407600",
    "https://www.redfin.com/CA/Los-Angeles/Boulevard-on-Wilshire/apartment/17242451",
    "https://www.redfin.com/CA/Pacific-Palisades/17326-Tramonto-Dr-90272/unit-403/home/183899165",
    "https://www.redfin.com/city/11203/CA/Los-Angeles/filter/property-type=townhouse,min-price=900k,max-price=8M,min-beds=2,min-baths=1",
    "https://www.redfin.com/city/11203/CA/Los-Angeles/apartments-for-rent/filter/property-type=house,min-price=500,max-price=4k,min-beds=0,min-baths=4"
  ],
  "includeExtras": false,
  "maxItems": 20,
  "endPage": 1,
  "customMapFunction": "(object) => { return {...object} }",
  "proxy": {
    "useApifyProxy": true
  }
}

```

## During the Run

During the run, the actor will output messages letting you know what is going on. Each message always contains a short label specifying which page from the provided list is currently specified.
When items are loaded from the page, you should see a message about this event with a loaded item count and total item count for each page.

If you provide incorrect input to the actor, it will immediately stop with a failure state and output an explanation of what is wrong.

## Redfin Export

During the run, the actor stores results into a dataset. Each item is a separate item in the dataset.

You can manage the results in any language (Python, PHP, Node JS/NPM). See the FAQ or <a href="https://www.apify.com/docs/api" target="blank">our API reference</a> to learn more about getting results from this Redfin actor.

## Scraped Redfin Properties

The structure of each item in Redfin products looks like this:

### Properties

```json
{
  "type": "property",
  "propertyId": "7115515",
  "listingId": "187064014",
  "listingDisplayLevel": "ACCESSIBLE",
  "mlsId": "24-385237",
  "url": "https://www.redfin.com/CA/Los-Angeles/1547-N-Sierra-Bonita-Ave-90046/home/7115515",
  "dataSourceId": "40",
  "marketId": "3",
  "businessMarketId": "5",
  "mlsStatusId": "54",
  "servicePolicyId": "31",
  "listingMetadata": {
    "searchStatus": "ACTIVE",
    "listingType": "MLS"
  },
  "propertyType": "VACANT_LAND",
  "priceInfo": {
    "amount": "3595000",
    "displayLevel": "ACCESSIBLE",
    "priceType": "LISTING_PRICE",
    "homePrice": {
      "displayLevel": "ACCESSIBLE",
      "int64Value": "3595000"
    }
  },
  "sqftInfo": {
    "displayLevel": "ACCESSIBLE"
  },
  "photosInfo": {
    "photoRanges": [
      {
        "startPos": 0,
        "endPos": 6,
        "version": "0"
      }
    ],
    "primaryPhotoDisplayLevel": "ACCESSIBLE",
    "secondaryPhotoDisplayLevel": "ACCESSIBLE"
  },
  "daysOnMarket": {
    "daysOnMarket": "1",
    "timeOnRedfin": "94222.238s",
    "listingAddedDate": "2024-05-01T16:49:05.830Z",
    "displayLevel": "ACCESSIBLE"
  },
  "timezone": "US/Pacific",
  "yearBuilt": {
    "displayLevel": "ACCESSIBLE"
  },
  "lotSize": {
    "amount": "13487",
    "displayLevel": "ACCESSIBLE"
  },
  "hoaDues": {
    "displayLevel": "ACCESSIBLE"
  },
  "sashes": [
    {
      "sashTypeId": 7,
      "sashTypeName": "New",
      "sashTypeColor": "#2E7E36",
      "timeOnRedfin": "94222238"
    }
  ],
  "brokers": {},
  "lastSaleData": {
    "lastSoldDate": "2021-08-17T07:00:00Z"
  },
  "personalization": {},
  "insights": {},
  "showMlsId": false,
  "directAccessInfo": {
    "supportPhoneNumber": "N/A",
    "timeZone": {
      "id": 7,
      "timeZoneIdString": "US/Pacific",
      "olsonTimeZoneIdString": "America/Los_Angeles",
      "description": "Pacific Time in the United States"
    }
  },
  "bathInfo": {},
  "addressInfo": {
    "centroid": {
      "centroid": {
        "latitude": 34.0995491,
        "longitude": -118.3542357
      },
      "displayLevel": "ACCESSIBLE"
    },
    "formattedStreetLine": "1547 N Sierra Bonita Ave",
    "city": "Los Angeles",
    "state": "CA",
    "zip": "90046",
    "location": "Hollywood",
    "streetlineDisplayLevel": "ACCESSIBLE",
    "unitNumberDisplayLevel": "ACCESSIBLE",
    "locationDisplayLevel": "ACCESSIBLE",
    "countryCode": "UNITED_STATES",
    "postalCodeDisplayLevel": "ACCESSIBLE"
  },
  "walkScore": {
    "walkScoreData": {
      "walkScore": {
        "value": 89,
        "link": "https://www.walkscore.com/score/1547+Sierra+Bonita+Ave+Los+Angeles+CA+90046/lat=34.0995491/lng=-118.3542357?utm_source=redfin",
        "shortDescription": "Very Walkable",
        "description": "Most errands can be accomplished on foot",
        "color": "#008000"
      },
      "bikeScore": {
        "value": 43,
        "link": "https://www.walkscore.com/score/1547+Sierra+Bonita+Ave+Los+Angeles+CA+90046/lat=34.0995491/lng=-118.3542357?utm_source=redfin"
      },
      "transitScore": {
        "value": 56,
        "link": "https://www.walkscore.com/score/1547+Sierra+Bonita+Ave+Los+Angeles+CA+90046/lat=34.0995491/lng=-118.3542357?utm_source=redfin"
      }
    }
  },
  "parcelBounds": {
    "bounds": [
      [
        "34.099501091596,-118.354041593315",
        "34.0995044615439,-118.354487111113",
        "34.0996418553602,-118.354487627678",
        "34.0996384853203,-118.354042142184",
        "34.099501091596,-118.354041593315"
      ]
    ],
    "fipsCode": "06037",
    "apn": "5550009004",
    "parcelLid": "02072AXHVCY0TXG1NX3ZRM",
    "polygon": "POLYGON ((-118.354041593315 34.099501091596,-118.354487111113 34.0995044615439,-118.354487627678 34.0996418553602,-118.354042142184 34.0996384853203,-118.354041593315 34.099501091596))"
  },
  "aboveTheFold": {
    "mainHouseInfo": {
      "listingId": 187064014,
      "openHousesDisplayLevel": 1,
      "videoOpenHouses": [],
      "hotnessInfo": {
        "isHot": false,
        "hotnessMessageInfoWithTourLink": {
          "hotnessMessageAction": "go tour it now"
        }
      },
      "listingAgents": [
        {
          "agentInfo": {
            "agentName": "Daniel Jacobson",
            "isAgentNameBlank": false,
            "isRedfinAgent": false,
            "isPartnerAgent": false,
            "isExternalAgent": false
          },
          "brokerName": "Compass",
          "license": "01963669",
          "licenseLabel": "DRE #",
          "breNumber": "01963669",
          "agentPhoneNumber": {
            "phoneNumber": "323-346-9111",
            "displayLevel": 1
          },
          "agentEmailAddress": "Djacobson.re@gmail.com",
          "isOpendoor": false
        }
      ],
      "buyingAgents": [],
      "remarksDisplayLevel": 1,
      "marketingRemarks": [
        {
          "marketingRemark": "This is a rare opportunity to acquire side-by-side lots in the sought-after Historical Sunset Square (HPOZ) area, with a combined area of 13,487 sqft and zoned LAR1. This sale encompasses the land at 1547 - 1551 N. Sierra Bonita, two addresses  &  two APN's. With the option to construct a single residence with ample outdoor space or develop two separate homes on each lot. Drive by and make an offer. AP'N : 5550009006  &  5550009004 ",
          "displayLevel": 1
        }
      ],
      "selectedAmenities": [
        {
          "header": "Community",
          "content": "Hollywood",
          "displayLevel": 1
        }
      ],
      "searchStatus": 1,
      "mlsStatusDisplay": {
        "displayValue": "Active",
        "definition": "This home is for sale and the sellers are accepting offers.",
        "longerDefinitionToken": "active"
      },
      "showPriceHomeLink": false,
      "showClaimHomeLink": false,
      "alwaysShowAgentAttribution": false,
      "showOffMarketWarning": false,
      "propertyAddress": {
        "streetNumber": "1547",
        "directionalPrefix": "N",
        "streetName": "Sierra Bonita",
        "streetType": "Ave",
        "directionalSuffix": "",
        "unitType": "",
        "unitValue": "",
        "city": "Los Angeles",
        "stateOrProvinceCode": "CA",
        "postalCode": "90046",
        "countryCode": "US"
      },
      "propertyIsActivish": true,
      "isComingSoonListing": false,
      "hasOfferDeadlineInEffect": false,
      "isOpendoorEligible": false,
      "isBDXEligible": false,
      "isAiChatEligible": true,
      "isDirectNewConstruction": false,
      "isFMLS": false,
      "propertyIsPremier": true,
      "doNotElevatePremierAgentCard": false,
      "customerAgentIsPremier": false,
      "customerAgentStatus": 1,
      "isLikelyUnderContractOrRecentlySold": false
    },
    "mediaBrowserInfo": {
      "scans": [],
      "videos": [],
      "isHot": false,
      "previousListingPhotosCount": 0
    },
    "openHouseInfo": {
      "openHouseList": [],
      "openHouseConfirmationIdMap": {},
      "openHouseCalendarInformationMap": {}
    },
    "photoTags": {
      "listingId": 187064014,
      "tagsByPhotoId": {},
      "includedFilterTags": {}
    }
  },
  "bannerData": [],
  "sellsideThreshold": {
    "__root": {
      "__g_id": "1417745292",
      "mustConfirmPartnerService": false,
      "redfinNowEligibility": {
        "__g_id": "890378136",
        "eligible": false
      },
      "propertyZip": "90046",
      "hasConcierge": true,
      "sellsideThreshold": 5,
      "businessMarketPhone": "+13107766327",
      "propertyId": 7115515,
      "hasOnePercentListingFee": false,
      "isOutOfService": false
    }
  },
  "belowTheFold": {
    "schoolsInfo": {
      "elementarySchools": [
        {
          "servesHome": true,
          "greatSchoolsRating": 6,
          "parentRating": 4,
          "distanceInMiles": "0.1",
          "gradeRanges": "K-5",
          "institutionType": "Public",
          "name": "Gardner Street Elementary School",
          "schoolUrl": "/school/186026/CA/Los-Angeles/Gardner-Street-Elementary-School",
          "searchUrl": "/school/186026/CA/Los-Angeles/Gardner-Street-Elementary-School",
          "greatSchoolOverviewUrl": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/",
          "id": 186026,
          "numberOfStudents": 408,
          "fullAddress": "7450 Hawthorn Ave, Los Angeles, CA 90046",
          "numReviews": 61,
          "studentToTeacherRatio": 25,
          "websiteUrl": "http://www.gardnerstreetschool.org",
          "schoolReviews": [
            {
              "schoolId": 39556114,
              "reviewedBy": "Parent",
              "datePosted": "August 2022",
              "review": "Principal and staff are ignorant. My child learned nothing",
              "reviewerRating": 1,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 5570031,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            }
          ],
          "schoolGranularRatings": [
            {
              "schoolId": 285686,
              "dataSourceId": 64,
              "schoolGranularRatingType": "Test Scores",
              "schoolGranularRatingValue": "8",
              "schoolGranularRatingCopyText": "State test results at this school are above the state average, so students are likely performing at or above grade level.\n\nBecause state averages can be low, some students at this school may still not be performing at grade level.\n",
              "schoolGranularRatingsDescription": "The Test Score Rating examines how students at this school performed on standardized tests compared with other schools in the state. The Test Rating was created using 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2022 California Science Test data from California Department of Education."
            }
          ],
          "schoolDistrict": {
            "id": 9940,
            "districtName": "Los Angeles Unified School District",
            "address": "333 South Beaudry Avenue",
            "city": "Los Angeles",
            "stateCode": "CA",
            "zip": "90017",
            "latitude": 34.056198,
            "longitude": -118.257172,
            "websiteUrl": "http://www.lausd.net",
            "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
          },
          "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/#Reviews",
          "elementary": true,
          "middle": false,
          "high": false,
          "lastUpdatedDate": "October 2023",
          "hasMultipleCatchmentAreas": false
        }
      ],
      "middleSchools": [
        {
          "servesHome": true,
          "greatSchoolsRating": 4,
          "parentRating": 4,
          "distanceInMiles": "1.3",
          "gradeRanges": "6-8",
          "institutionType": "Public",
          "name": "Hubert Howe Bancroft Middle School",
          "schoolUrl": "/school/187028/CA/Los-Angeles/Hubert-Howe-Bancroft-Middle-School",
          "searchUrl": "/school/187028/CA/Los-Angeles/Hubert-Howe-Bancroft-Middle-School",
          "greatSchoolOverviewUrl": "https://www.greatschools.org/california/los-angeles/1918-Hubert-Howe-Bancroft-Middle-School/",
          "id": 187028,
          "numberOfStudents": 742,
          "fullAddress": "929 N Las Palmas Ave, Los Angeles, CA 90038",
          "numReviews": 26,
          "studentToTeacherRatio": 22,
          "websiteUrl": "http://www.bancroftmiddleschool.org",
          "schoolReviews": [
            {
              "schoolId": 39554971,
              "reviewedBy": "Student",
              "datePosted": "June 2023",
              "review": "I have no clue what the others are saying or what the results in this webpage are but it is very inaccurate. This is an amazing school, especially for STEM. The kids here are actually ABOVE average in terms of intelligence. As someone who attended the school 6-8th, I can have a say that the staff are friendly and teach in ways supportive to education. I am already college ready.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 601918,
              "maponicsId": 5986997,
              "url": "https://www.greatschools.org/california/los-angeles/1918-Hubert-Howe-Bancroft-Middle-School/"
            },
            {
              "schoolId": 39554970,
              "reviewedBy": "Other",
              "datePosted": "June 2022",
              "review": "HORRIBLE! The kids there are so disrespectful and rude. They are not excepting of anyone, all they do is make the teachers cry and jump on desks and you can’t even learn anything. and people will put sticky notes on ur back and slam u into lockers! DO NOT SEND YOUR KIDS HERE!",
              "reviewerRating": 1,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 601918,
              "maponicsId": 5478109,
              "url": "https://www.greatschools.org/california/los-angeles/1918-Hubert-Howe-Bancroft-Middle-School/"
            },
            {
              "schoolId": 39554969,
              "reviewedBy": "Student",
              "datePosted": "December 2019",
              "review": "I love this school. Because I also saw some of my old friends. And I make new friends here. They have SPED. I recommended you coming here and trying out this school.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 601918,
              "maponicsId": 4334242,
              "url": "https://www.greatschools.org/california/los-angeles/1918-Hubert-Howe-Bancroft-Middle-School/"
            },
            {
              "schoolId": 39554968,
              "reviewedBy": "Parent",
              "datePosted": "October 2019",
              "review": "This school has really helped my daughter with her skills and the STEAM program, has allowed challenging curriculum. They do a great job integrating art and design. Her writing skills improved too!",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 601918,
              "maponicsId": 4260919,
              "url": "https://www.greatschools.org/california/los-angeles/1918-Hubert-Howe-Bancroft-Middle-School/"
            },
            {
              "schoolId": 39554967,
              "reviewedBy": "Parent",
              "datePosted": "October 2019",
              "review": "Bancroft instilled a love of engineering and design in our son, like nothing else. It gave him a passion to pursue and inspired him to aim for the stars. He will be graduating high school with a 4.0, and without a doubt, it all started at Bancroft. Thank you for inspiring him to reach for the stars.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 601918,
              "maponicsId": 4260845,
              "url": "https://www.greatschools.org/california/los-angeles/1918-Hubert-Howe-Bancroft-Middle-School/"
            },
            {
              "schoolId": 39554966,
              "reviewedBy": "Parent",
              "datePosted": "October 2019",
              "review": "Excellent School. Great support. Everyone, from the Principal to teachers to after-school coordinators are very caring and supportive.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 601918,
              "maponicsId": 4260145,
              "url": "https://www.greatschools.org/california/los-angeles/1918-Hubert-Howe-Bancroft-Middle-School/"
            },
            {
              "schoolId": 39554965,
              "reviewedBy": "Parent",
              "datePosted": "May 2019",
              "review": "For me was great school because the teachers are very good and the staff are very polite and my daughter is satisfied with the teachers and classes.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 601918,
              "maponicsId": 4003756,
              "url": "https://www.greatschools.org/california/los-angeles/1918-Hubert-Howe-Bancroft-Middle-School/"
            }
          ],
          "schoolGranularRatings": [
            {
              "schoolId": 287893,
              "dataSourceId": 64,
              "schoolGranularRatingType": "Test Scores",
              "schoolGranularRatingValue": "3",
              "schoolGranularRatingCopyText": "State test results at this school are below the state average, so students are likely performing below grade level.",
              "schoolGranularRatingsDescription": "The Test Score Rating examines how students at this school performed on standardized tests compared with other schools in the state. The Test Rating was created using 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2022 California Science Test data from California Department of Education."
            },
            {
              "schoolId": 287891,
              "dataSourceId": 64,
              "schoolGranularRatingType": "Equity",
              "schoolGranularRatingValue": "3",
              "schoolGranularRatingCopyText": "Underserved students at this school may be falling behind other students in the state, and this school may have achievement gaps between different student groups.",
              "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
            },
            {
              "schoolId": 287894,
              "dataSourceId": 64,
              "schoolGranularRatingType": "Academic Progress",
              "schoolGranularRatingValue": "6",
              "schoolGranularRatingCopyText": "Students at this school are making average academic progress from one grade to the next compared to similar students in the state.",
              "schoolGranularRatingsDescription": "The Academic Progress Rating is a growth proxy rating based on a model using unmatched cohorts, or school-level data instead of student-level data. The Academic Progress Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2019 demographic data from California Department of Education."
            }
          ],
          "schoolDistrict": {
            "id": 9940,
            "districtName": "Los Angeles Unified School District",
            "address": "333 South Beaudry Avenue",
            "city": "Los Angeles",
            "stateCode": "CA",
            "zip": "90017",
            "latitude": 34.056198,
            "longitude": -118.257172,
            "websiteUrl": "http://www.lausd.net",
            "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
          },
          "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/los-angeles/1918-Hubert-Howe-Bancroft-Middle-School/#Reviews",
          "elementary": false,
          "middle": true,
          "high": false,
          "lastUpdatedDate": "October 2023",
          "hasMultipleCatchmentAreas": false
        }
      ],
      "highSchools": [
        {
          "servesHome": true,
          "greatSchoolsRating": 6,
          "parentRating": 4,
          "distanceInMiles": "0.8",
          "gradeRanges": "9-12",
          "institutionType": "Public",
          "name": "Hollywood Senior High School",
          "schoolUrl": "/school/92138/CA/Los-Angeles/Hollywood-Senior-High-School",
          "searchUrl": "/school/92138/CA/Los-Angeles/Hollywood-Senior-High-School",
          "greatSchoolOverviewUrl": "https://www.greatschools.org/california/los-angeles/2148-Hollywood-Senior-High-School/",
          "id": 92138,
          "numberOfStudents": 1481,
          "fullAddress": "1521 N Highland Ave, Los Angeles, CA 90028",
          "numReviews": 24,
          "studentToTeacherRatio": 26,
          "websiteUrl": "http://www.hollywoodhighschool.net",
          "schoolReviews": [
            {
              "schoolId": 39556369,
              "reviewedBy": "Parent",
              "datePosted": "April 2019",
              "review": "I regret sending my child here. The administration particularly is slowly eliminating anything positive like sports programs. Read the article in the LA Times about having all coaches reapplying for their positions, successful and not. A disgrace.",
              "reviewerRating": 2,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602148,
              "maponicsId": 3986499,
              "url": "https://www.greatschools.org/california/los-angeles/2148-Hollywood-Senior-High-School/"
            },
            {
              "schoolId": 39556368,
              "reviewedBy": "Student",
              "datePosted": "July 2018",
              "review": "I did not like my experience here academically. The students are highly unmotivated to learn and disrespectful. The only great thing about this school is the musical theatre program. Many dedicated, passionate and hardworking students participate in the musical production. There is still some drama involved in it though, and the Performing Arts Magnet is full of students who don't care about the performing arts. There are some, but they are far and few. The School for Advanced Studies is a bit better academically, but still the students are disrespectful and chatty. I DO NOT recommend this school.",
              "reviewerRating": 2,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602148,
              "maponicsId": 3504296,
              "url": "https://www.greatschools.org/california/los-angeles/2148-Hollywood-Senior-High-School/"
            },
            {
              "schoolId": 8515694,
              "reviewedBy": "Student",
              "datePosted": "August 2014",
              "review": "Hollywood High is amazing! I am in the School for Advanced Studies at Hollywood and my teachers are all great, they care and treat you like a human. As well as having an amazing SAS program Hollywood also has an OUTSTANDING Performing Arts Magnet. I was blown away the first time I saw students from PAM performing because they truly are amazing and their instructors and choreographers are genius's. I've said all these amazing things about my school, but I haven't even said anything about the counselors yet! Everyone is great, but by far the most amazing person is Mrs. Brown. Not only is she the SAS counselor, but also the College counselor, the 9/10th grade counselor and someone who will respond to you and doesn't just disregard your concerns. I've met plenty of counselors who you have to hunt down to get a concern in and then still worry if it's actually going to be taken care of, but I can promise you that won't be the case at Hollywood. We have Extraordinary SAS, Outstanding PAM, Amazing administration and to too it all off; there is so much school spirit, enthusiasm and every student I have seen at Hollywood is an individual. Can you find a better high school?",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602148,
              "maponicsId": 1518926,
              "url": "https://www.greatschools.org/california/los-angeles/2148-Hollywood-Senior-High-School/"
            },
            {
              "schoolId": 8515693,
              "reviewedBy": "Student",
              "datePosted": "April 2014",
              "review": "I am now a 10th grader here and i am loving it. I had to transfer from my last school (terrible). Anyways, this school has by far the best PAM program which are for the preforming arts which i attend. This program attaches to the best dances, music, acting, and singing courses. For dance ask for Mrs.Goldschein for acting Mrs.Bridges for singing Mr.Sexton for Music Mrs. Hall all teachers are loving and caring. Myself I'm an Opera Singer in training and by most this school is diverse and loving much to look forward to.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602148,
              "maponicsId": 1472027,
              "url": "https://www.greatschools.org/california/los-angeles/2148-Hollywood-Senior-High-School/"
            },
            {
              "schoolId": 14510590,
              "reviewedBy": "Teacher",
              "datePosted": "September 2013",
              "review": "Hollywood High School Performing Arts Magnet is one of the best magnets in the city, with its amazing staff of talented teachers and directors, and its rigorous academic classes. Our school can proudly claim many Ph.d's from Stanford and other highly regarded universities, and teachers with professional experience in top ranked industries. The school is cozy and intimate with a small student to teacher ratio which insures a great deal of individual attention and care for the individual. This is a school with a long and astounding legacy - Judy Garland and Carol Burnett walked its halls, along with great luminaries in law and politics, such as Warren Christopher. Today we are still preparing future great leaders of the world, for many of our students go on to prestigious universities such as Berkeley, New York University and U.C.L.A. Hollywood is a school where one can truly receive a great education.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602148,
              "maponicsId": 1380048,
              "url": "https://www.greatschools.org/california/los-angeles/2148-Hollywood-Senior-High-School/"
            },
            {
              "schoolId": 5353951,
              "reviewedBy": "Parent",
              "datePosted": "August 2013",
              "review": "I admire very much the Magnet Program and especially the arts part. Those teachers involved in Performing Arts are excellent, they put heart and soul in it. I have a big admiration for their hard work which reflects in the shows the students perform. Good job.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602148,
              "maponicsId": 1367285,
              "url": "https://www.greatschools.org/california/los-angeles/2148-Hollywood-Senior-High-School/"
            },
            {
              "schoolId": 5353950,
              "reviewedBy": "Student",
              "datePosted": "August 2013",
              "review": "This school is small and boring and full of lies from staff and doesnt give you the oppurtunity you want.",
              "reviewerRating": 3,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602148,
              "maponicsId": 1357336,
              "url": "https://www.greatschools.org/california/los-angeles/2148-Hollywood-Senior-High-School/"
            }
          ],
          "schoolGranularRatings": [
            {
              "schoolId": 360954,
              "dataSourceId": 64,
              "schoolGranularRatingType": "College Readiness",
              "schoolGranularRatingValue": "7",
              "schoolGranularRatingCopyText": "This school is above the state average in key measures of college and career readiness, such as graduation rates, college entrance exams, and advanced courses.\n\nEven at schools with strong college and career readiness, there may be students who are not getting the opportunities they need to succeed.\n",
              "schoolGranularRatingsDescription": "The College Readiness Rating uses this high school's graduation rates, college entrance exam participation and performance, or AP, IB, or Dual Enrollment participation to determine how well schools are preparing students for success in college and beyond. The College Readiness Rating was created using the following data from the 2018 Civil Rights Data Collection: percentage of students enrolled in IB, AP or Dual Enrollment classes in grades 9-12, using 2019 SAT percent college ready data from California Department of Education, using 2019 Average ACT score data from California Department of Education, using 2019 ACT percent college ready data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
            },
            {
              "schoolId": 303709,
              "dataSourceId": 64,
              "schoolGranularRatingType": "Equity",
              "schoolGranularRatingValue": "6",
              "schoolGranularRatingCopyText": "Underserved students at this school are performing about as well as other students in the state, but this school may still have achievement gaps between different student groups.",
              "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
            },
            {
              "schoolId": 303708,
              "dataSourceId": 64,
              "schoolGranularRatingType": "Test Scores",
              "schoolGranularRatingValue": "4",
              "schoolGranularRatingCopyText": "State test results at this school are below the state average, so students are likely performing below grade level.",
              "schoolGranularRatingsDescription": "The Test Score Rating examines how students at this school performed on standardized tests compared with other schools in the state. The Test Rating was created using 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2022 California Science Test data from California Department of Education."
            },
            {
              "schoolId": 303706,
              "dataSourceId": 64,
              "schoolGranularRatingType": "Academic Progress",
              "schoolGranularRatingValue": "7",
              "schoolGranularRatingCopyText": "Students at this school are making more academic progress from one grade to the next compared to similar students in the state.",
              "schoolGranularRatingsDescription": "The Academic Progress Rating is a growth proxy rating based on a model using unmatched cohorts, or school-level data instead of student-level data. The Academic Progress Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2019 demographic data from California Department of Education."
            }
          ],
          "schoolDistrict": {
            "id": 9940,
            "districtName": "Los Angeles Unified School District",
            "address": "333 South Beaudry Avenue",
            "city": "Los Angeles",
            "stateCode": "CA",
            "zip": "90017",
            "latitude": 34.056198,
            "longitude": -118.257172,
            "websiteUrl": "http://www.lausd.net",
            "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
          },
          "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/los-angeles/2148-Hollywood-Senior-High-School/#Reviews",
          "elementary": false,
          "middle": false,
          "high": true,
          "lastUpdatedDate": "October 2023",
          "hasMultipleCatchmentAreas": false
        },
      ],
      "servingThisHomeSchools": [
        {
          "servesHome": true,
          "greatSchoolsRating": 6,
          "parentRating": 4,
          "distanceInMiles": "0.1",
          "gradeRanges": "K-5",
          "institutionType": "Public",
          "name": "Gardner Street Elementary School",
          "schoolUrl": "/school/186026/CA/Los-Angeles/Gardner-Street-Elementary-School",
          "searchUrl": "/school/186026/CA/Los-Angeles/Gardner-Street-Elementary-School",
          "greatSchoolOverviewUrl": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/",
          "id": 186026,
          "numberOfStudents": 408,
          "fullAddress": "7450 Hawthorn Ave, Los Angeles, CA 90046",
          "numReviews": 61,
          "studentToTeacherRatio": 25,
          "websiteUrl": "http://www.gardnerstreetschool.org",
          "schoolReviews": [
            {
              "schoolId": 39556114,
              "reviewedBy": "Parent",
              "datePosted": "August 2022",
              "review": "Principal and staff are ignorant. My child learned nothing",
              "reviewerRating": 1,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 5570031,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556113,
              "reviewedBy": "Parent",
              "datePosted": "August 2022",
              "review": "Amazing school! My son went to Gardner street school from pre-K to 5th grade and was the best; great teachers, very involved and committed. Great sense of community.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 5547784,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556112,
              "reviewedBy": "Parent",
              "datePosted": "February 2020",
              "review": "My husband and I assumed that we will have to go Private for our son's Elementary education, because of the urban myth of \"there are no good public schools in Hollywood\". As we were touring all the private schools in the area, we decided to stop by the Open House at our zoned public school, Gardner - just to see for ourselves how much worse it would be. We were shocked to find that we loved everything about Gardner! The way it looks and feels (happy! Filled with art, music and laughter), and mostly its progressive mindset. We were so shaken up that we went back two more times to confirm that our initial impression was not skewed in some way... Our son is now a thriving 2nd grader at Gardner, and we truly do love this school.Gardner Street Elementary is a neighborhood gem with an engaged principal, excellent teachers and helpful staff, and a small but very active parent body that makes things happen. Many students live outside the school zone but they and their parents like Gardner so much that they've chosen to permit in.What makes Gardner so special are its diverse staff and students, family-like atmosphere, and the many \"extras\". Thanks to annual fundraisers coupled with lots of fun events for kids and families throughout the school year, \"non-essential\" classes that are not paid for by LAUSD and not offered in many public schools - such as Visual Art, Music, PE, Yoga, Organic Edible Garden, and (new!) Science Lab - are part of the everyday curriculum and available to every child at Gardner. There are many really great afterschool programs to choose from, and occasional workshops for parents/caregivers. Campus beautification is ongoing.Besides all the fun, there is serious learning going on as well. Learning to truly *understand* and not just memorize to ace tests. So much learning goes on at school that - as supported by countless studies and best-performing schools in Finland - there are no homework requirements for the younger kids.Most importantly, my child has developed love for reading and learning, he is excited about going to school every day, and he feels safe there.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 4418512,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556111,
              "reviewedBy": "Parent",
              "datePosted": "September 2019",
              "review": "We just love Gardner and couldn't have had a better experience. The emphasis on strong values is just amazing and all the kids are so kind! We've found the teachers and admins to be really open to differently-abled kids and their needs. We feel very lucky to have found Gardner!",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 4226639,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556110,
              "reviewedBy": "Parent",
              "datePosted": "September 2019",
              "review": "Originally, we were planning on going to private school then we checked out Gardner and it has turned out awesome.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 4225875,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556109,
              "reviewedBy": "Parent",
              "datePosted": "May 2019",
              "review": "We chose to go to Gardner as a transfer family. When we walked in it felt different than other schools. It was clean, people were friendly and it felt HAPPY! There were happy children around everywhere. It was bright and colorful and pleasant to be in. We have had good teachers, met wonderful parents, and became very involved. The school has a great group of parents running the parent group that raises money all year for the programs that LAUSD doesn't provide. PE, Music, Art, Yoga and Edible Gardening are all only provided because the parents raise the money. And it's not a crazy amount. They aren't hounding you for money. It's usually through fun events like carnivals or dances. They encourage a monthly donation of as little as 5-10 dollars a month and it's totally doable and worth it for what the kids get in the end. Parents don't realize that other public schools don't offer those programs. But Gardner students are thriving thanks to them. We believe in public schools. And LAUSD has a pretty rough reputation, but this is one of those schools that's a diamond in the rough. It is diverse both racially and economically. It's has three main languages spoken among students. It has loving and respectful teachers and administration. Parents that are involved and creating an enriching environment for kids and helping to create a healthier education for them. It's a great size and very inclusive and we are THRILLED to be a part of it.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 4061327,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556108,
              "reviewedBy": "Parent",
              "datePosted": "May 2019",
              "review": "I’m running from this school since we day one. Schools are what you make of it but no ..... unless you pay them for alllll the donations and your attending each bake sale they have an entitled , nasty , belittling attitude. There is no diversity at least with African American or mixed race . They will accent your child but they definitely have the attitude of go else where why are you here.",
              "reviewerRating": 1,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 4056201,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            }
          ],
          "schoolGranularRatings": [
            {
              "schoolId": 285686,
              "dataSourceId": 64,
              "schoolGranularRatingType": "Test Scores",
              "schoolGranularRatingValue": "8",
              "schoolGranularRatingCopyText": "State test results at this school are above the state average, so students are likely performing at or above grade level.\n\nBecause state averages can be low, some students at this school may still not be performing at grade level.\n",
              "schoolGranularRatingsDescription": "The Test Score Rating examines how students at this school performed on standardized tests compared with other schools in the state. The Test Rating was created using 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2022 California Science Test data from California Department of Education."
            },
            {
              "schoolId": 285685,
              "dataSourceId": 64,
              "schoolGranularRatingType": "Equity",
              "schoolGranularRatingValue": "5",
              "schoolGranularRatingCopyText": "Underserved students at this school are performing about as well as other students in the state, but this school may still have achievement gaps between different student groups.",
              "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
            },
            {
              "schoolId": 285687,
              "dataSourceId": 64,
              "schoolGranularRatingType": "Academic Progress",
              "schoolGranularRatingValue": "6",
              "schoolGranularRatingCopyText": "Students at this school are making average academic progress from one grade to the next compared to similar students in the state.",
              "schoolGranularRatingsDescription": "The Academic Progress Rating is a growth proxy rating based on a model using unmatched cohorts, or school-level data instead of student-level data. The Academic Progress Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2019 demographic data from California Department of Education."
            }
          ],
          "schoolDistrict": {
            "id": 9940,
            "districtName": "Los Angeles Unified School District",
            "address": "333 South Beaudry Avenue",
            "city": "Los Angeles",
            "stateCode": "CA",
            "zip": "90017",
            "latitude": 34.056198,
            "longitude": -118.257172,
            "websiteUrl": "http://www.lausd.net",
            "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
          },
          "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/#Reviews",
          "elementary": true,
          "middle": false,
          "high": false,
          "lastUpdatedDate": "October 2023",
          "hasMultipleCatchmentAreas": false
        }
      ],
      "totalSchoolsServiced": 4,
      "sectionPreviewText": "Average rating 5.3 out of 10",
      "shouldHideSection": false
    },
    "publicRecordsInfo": {
      "basicInfo": {
        "beds": 5,
        "baths": 2,
        "propertyTypeName": "Single Family Residential",
        "numStories": 2,
        "yearBuilt": 1913,
        "yearRenovated": 1913,
        "sqFtFinished": 3152,
        "totalSqFt": 3152,
        "lotSqFt": 6745,
        "apn": "5550009004",
        "propertyLastUpdatedDate": 1712988490999,
        "displayTimeZone": "US/Pacific"
      },
      "taxInfo": {
        "taxableLandValue": 1326000,
        "taxableImprovementValue": 331500,
        "rollYear": 2023,
        "taxesDue": 20288.64
      },
      "allTaxInfo": [
        {
          "taxableLandValue": 1326000,
          "taxableImprovementValue": 331500,
          "rollYear": 2023,
          "taxesDue": 20288.64
        }
      ],
      "addressInfo": {
        "isFMLS": false,
        "street": "1547 N Sierra Bonita Ave",
        "city": "Los Angeles",
        "state": "CA",
        "zip": "90046",
        "countryCode": "US"
      },
      "mortgageCalculatorInfo": {
        "displayLevel": 1,
        "dataSourceId": 40,
        "listingPrice": 3595000,
        "downPaymentPercentage": 20,
        "propertyTaxRate": 1.25,
        "homeInsuranceRate": 0.32,
        "mortgageInsuranceRate": 0.75,
        "creditScore": 740,
        "loanType": 1,
        "mortgageRateInfo": {
          "tenYearFixed": 6.798,
          "fifteenYearFixed": 6.924,
          "twentyYearFixed": 7.547,
          "thirtyYearFixed": 7.708,
          "vaThirtyYearFixed": 7.235,
          "fhaThirtyYearFixed": 7.176,
          "threeYearArm": 6.844,
          "fiveOneArm": 7.215,
          "sevenYearArm": 7.399,
          "tenYearArm": 7.488,
          "tenYearFixedJumbo": 7.192,
          "fifteenYearFixedJumbo": 7.248,
          "twentyYearFixedJumbo": 7.479,
          "thirtyYearFixedJumbo": 7.586,
          "vaThirtyYearFixedJumbo": 7.313,
          "threeYearArmJumbo": 7.358,
          "fiveOneArmJumbo": 7.145,
          "sevenYearArmJumbo": 7.179,
          "tenYearArmJumbo": 7.33,
          "isFromBankrate": true
        },
        "bankrateDisclaimers": {
          "thirtyYearFixedDisclaimer": "According to icanbuy, the average interest rate for a 30 Year Fixed loan in 90046 is 7.708%.",
          "fifteenYearFixedDisclaimer": "According to icanbuy, the average interest rate for a 15 Year Fixed loan in 90046 is 6.924%.",
          "fiveOneArmDisclaimer": "According to icanbuy, the average interest rate for a 5/1 ARM loan in 90046 is 7.215%."
        },
        "countyId": 321,
        "stateId": 9,
        "countyName": "Los Angeles County",
        "stateName": "California",
        "mortgageRatesPageLinkText": "View all rates",
        "baseMortgageRatesPageURL": "/mortgage-rates?location=90046&locationType=4&locationId=37493",
        "zipCode": "90046",
        "isCoop": false,
        "mortgageConformingLoanInfo": {
          "oneUnitLimit": 1149825,
          "twoUnitLimit": 1472250,
          "threeUnitLimit": 1779525,
          "fourUnitLimit": 2211600
        }
      },
      "latestListingInfo": {
        "lotSqFt": 13487,
        "propertyTypeName": "Vacant Land"
      },
      "countyUrl": "/county/321/CA/Los-Angeles-County",
      "countyName": "Los Angeles County",
      "countyIsActive": true,
      "sectionPreviewText": "Compare listing to public record, refreshed 04/12/2024"
    },
    "propertyHistoryInfo": {
      "isHistoryStillGrowing": false,
      "hasAdminContent": false,
      "hasLoginContent": false,
      "dataSourceId": 40,
      "canSeeListing": true,
      "listingIsNull": false,
      "hasPropertyHistory": true,
      "showLogoInLists": false,
      "definitions": [
        {
          "term": "Multi-Property Sale",
          "slug": "multi-property-sale",
          "link": "/definition/multi-property-sale",
          "brief": "A sale in which more than one property was purchased simultaneously, resulting in a purchase price that may not accurately reflect the real value of the property."
        }
      ],
      "displayTimeZone": "US/Pacific",
      "isAdminOnlyView": false,
      "events": [
        {
          "isEventAdminOnly": false,
          "price": 3595000,
          "isPriceAdminOnly": false,
          "eventDescription": "Listed",
          "mlsDescription": "Active",
          "source": "TheMLS",
          "sourceId": "24-385237",
          "dataSourceDisplay": {
            "dataSourceId": 40,
            "dataSourceDescription": "Combined LA/Westside MLS (CLAW TheMLS)",
            "dataSourceName": "TheMLS",
            "dataSourceImage": "claw_small.png",
            "shouldShowLargerLogo": false
          },
          "priceDisplayLevel": 1,
          "historyEventType": 1,
          "eventDate": 1714460400000
        }
      ],
      "mediaBrowserInfoBySourceId": {
        "21-748892": {
          "photos": [
            {
              "photoUrls": {
                "nonFullScreenPhotoUrlCompressed": "https://ssl.cdn-redfin.com/photo/40/mbphoto/892/genMid.21-748892_0.jpg",
                "nonFullScreenPhotoUrl": "https://ssl.cdn-redfin.com/photo/40/mbpaddedwide/892/genMid.21-748892_0.jpg",
                "fullScreenPhotoUrl": "https://ssl.cdn-redfin.com/photo/40/bigphoto/892/21-748892_0.jpg",
                "lightboxListUrl": "https://ssl.cdn-redfin.com/photo/40/bcsphoto/892/genBcs.21-748892_0.jpg"
              },
              "thumbnailData": {
                "thumbnailUrl": "https://ssl.cdn-redfin.com/photo/40/tmbphoto/892/genTmb.21-748892_0.jpg"
              },
              "displayLevel": 1,
              "dataSourceId": 40,
              "photoType": "SPRITED",
              "subdirectory": "892",
              "fileName": "21-748892_0.jpg",
              "height": 646,
              "width": 1280,
              "photoId": 1552056202
            }
          ],
          "scans": [],
          "isHot": false,
          "streetView": {
            "latLong": {
              "latitude": 34.0995491,
              "longitude": -118.3542357
            },
            "streetViewUrl": "https://maps.googleapis.com/maps/api/streetview?channel=mb-ldp-publicrecord&location=1547+N+Sierra+Bonita+Ave%2C+Los+Angeles%2C+CA+90046&size=665x441&source=outdoor&client=gme-redfin&signature=LQ89hRxweNCApbl8LjVYm8Hh1eo=",
            "displayLevel": 1,
            "dataSourceId": 40,
            "streetViewAvailable": true
          },
          "altTextForImage": "1547 N Sierra Bonita Ave, Los Angeles, CA 90046",
          "assembledAddress": "1547 N Sierra Bonita Ave",
          "previousListingPhotosCount": 0,
          "displayType": 1
        }
      },
      "addressInfo": {
        "isFMLS": false,
        "street": "1547 N Sierra Bonita Ave",
        "city": "Los Angeles",
        "state": "CA",
        "zip": "90046",
        "countryCode": "US"
      },
      "isFMLS": false,
      "historyHasHiddenRows": false,
      "priceEstimates": {
        "displayLevel": 1,
        "priceHomeUrl": "/what-is-my-home-worth?estPropertyId=7115515&src=ldp-estimates"
      },
      "sectionPreviewText": "Last sold on August 17, 2021 for $2,300,000",
      "timelineDisplayLevel": 1
    },
    "schoolsAndDistrictsInfo": {
      "elementarySchools": [
        {
          "servesHome": true,
          "greatSchoolsRating": 6,
          "parentRating": 4,
          "distanceInMiles": "0.1",
          "gradeRanges": "K-5",
          "institutionType": "Public",
          "name": "Gardner Street Elementary School",
          "schoolUrl": "/school/186026/CA/Los-Angeles/Gardner-Street-Elementary-School",
          "searchUrl": "/school/186026/CA/Los-Angeles/Gardner-Street-Elementary-School",
          "greatSchoolOverviewUrl": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/",
          "id": 186026,
          "numberOfStudents": 408,
          "fullAddress": "7450 Hawthorn Ave, Los Angeles, CA 90046",
          "numReviews": 61,
          "studentToTeacherRatio": 25,
          "websiteUrl": "http://www.gardnerstreetschool.org",
          "schoolReviews": [
            {
              "schoolId": 39556114,
              "reviewedBy": "Parent",
              "datePosted": "August 2022",
              "review": "Principal and staff are ignorant. My child learned nothing",
              "reviewerRating": 1,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 5570031,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556113,
              "reviewedBy": "Parent",
              "datePosted": "August 2022",
              "review": "Amazing school! My son went to Gardner street school from pre-K to 5th grade and was the best; great teachers, very involved and committed. Great sense of community.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 5547784,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556112,
              "reviewedBy": "Parent",
              "datePosted": "February 2020",
              "review": "My husband and I assumed that we will have to go Private for our son's Elementary education, because of the urban myth of \"there are no good public schools in Hollywood\". As we were touring all the private schools in the area, we decided to stop by the Open House at our zoned public school, Gardner - just to see for ourselves how much worse it would be. We were shocked to find that we loved everything about Gardner! The way it looks and feels (happy! Filled with art, music and laughter), and mostly its progressive mindset. We were so shaken up that we went back two more times to confirm that our initial impression was not skewed in some way... Our son is now a thriving 2nd grader at Gardner, and we truly do love this school.Gardner Street Elementary is a neighborhood gem with an engaged principal, excellent teachers and helpful staff, and a small but very active parent body that makes things happen. Many students live outside the school zone but they and their parents like Gardner so much that they've chosen to permit in.What makes Gardner so special are its diverse staff and students, family-like atmosphere, and the many \"extras\". Thanks to annual fundraisers coupled with lots of fun events for kids and families throughout the school year, \"non-essential\" classes that are not paid for by LAUSD and not offered in many public schools - such as Visual Art, Music, PE, Yoga, Organic Edible Garden, and (new!) Science Lab - are part of the everyday curriculum and available to every child at Gardner. There are many really great afterschool programs to choose from, and occasional workshops for parents/caregivers. Campus beautification is ongoing.Besides all the fun, there is serious learning going on as well. Learning to truly *understand* and not just memorize to ace tests. So much learning goes on at school that - as supported by countless studies and best-performing schools in Finland - there are no homework requirements for the younger kids.Most importantly, my child has developed love for reading and learning, he is excited about going to school every day, and he feels safe there.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 4418512,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556111,
              "reviewedBy": "Parent",
              "datePosted": "September 2019",
              "review": "We just love Gardner and couldn't have had a better experience. The emphasis on strong values is just amazing and all the kids are so kind! We've found the teachers and admins to be really open to differently-abled kids and their needs. We feel very lucky to have found Gardner!",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 4226639,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556110,
              "reviewedBy": "Parent",
              "datePosted": "September 2019",
              "review": "Originally, we were planning on going to private school then we checked out Gardner and it has turned out awesome.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 4225875,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556109,
              "reviewedBy": "Parent",
              "datePosted": "May 2019",
              "review": "We chose to go to Gardner as a transfer family. When we walked in it felt different than other schools. It was clean, people were friendly and it felt HAPPY! There were happy children around everywhere. It was bright and colorful and pleasant to be in. We have had good teachers, met wonderful parents, and became very involved. The school has a great group of parents running the parent group that raises money all year for the programs that LAUSD doesn't provide. PE, Music, Art, Yoga and Edible Gardening are all only provided because the parents raise the money. And it's not a crazy amount. They aren't hounding you for money. It's usually through fun events like carnivals or dances. They encourage a monthly donation of as little as 5-10 dollars a month and it's totally doable and worth it for what the kids get in the end. Parents don't realize that other public schools don't offer those programs. But Gardner students are thriving thanks to them. We believe in public schools. And LAUSD has a pretty rough reputation, but this is one of those schools that's a diamond in the rough. It is diverse both racially and economically. It's has three main languages spoken among students. It has loving and respectful teachers and administration. Parents that are involved and creating an enriching environment for kids and helping to create a healthier education for them. It's a great size and very inclusive and we are THRILLED to be a part of it.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 4061327,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556108,
              "reviewedBy": "Parent",
              "datePosted": "May 2019",
              "review": "I’m running from this school since we day one. Schools are what you make of it but no ..... unless you pay them for alllll the donations and your attending each bake sale they have an entitled , nasty , belittling attitude. There is no diversity at least with African American or mixed race . They will accent your child but they definitely have the attitude of go else where why are you here.",
              "reviewerRating": 1,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 4056201,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            }
          ],
          "schoolGranularRatings": [
            {
              "schoolId": 285686,
              "dataSourceId": 64,
              "schoolGranularRatingType": "Test Scores",
              "schoolGranularRatingValue": "8",
              "schoolGranularRatingCopyText": "State test results at this school are above the state average, so students are likely performing at or above grade level.\n\nBecause state averages can be low, some students at this school may still not be performing at grade level.\n",
              "schoolGranularRatingsDescription": "The Test Score Rating examines how students at this school performed on standardized tests compared with other schools in the state. The Test Rating was created using 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2022 California Science Test data from California Department of Education."
            },
            {
              "schoolId": 285685,
              "dataSourceId": 64,
              "schoolGranularRatingType": "Equity",
              "schoolGranularRatingValue": "5",
              "schoolGranularRatingCopyText": "Underserved students at this school are performing about as well as other students in the state, but this school may still have achievement gaps between different student groups.",
              "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
            },
            {
              "schoolId": 285687,
              "dataSourceId": 64,
              "schoolGranularRatingType": "Academic Progress",
              "schoolGranularRatingValue": "6",
              "schoolGranularRatingCopyText": "Students at this school are making average academic progress from one grade to the next compared to similar students in the state.",
              "schoolGranularRatingsDescription": "The Academic Progress Rating is a growth proxy rating based on a model using unmatched cohorts, or school-level data instead of student-level data. The Academic Progress Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2019 demographic data from California Department of Education."
            }
          ],
          "schoolDistrict": {
            "id": 9940,
            "districtName": "Los Angeles Unified School District",
            "address": "333 South Beaudry Avenue",
            "city": "Los Angeles",
            "stateCode": "CA",
            "zip": "90017",
            "latitude": 34.056198,
            "longitude": -118.257172,
            "websiteUrl": "http://www.lausd.net",
            "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
          },
          "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/#Reviews",
          "elementary": true,
          "middle": false,
          "high": false,
          "lastUpdatedDate": "October 2023",
          "hasMultipleCatchmentAreas": false
        }
      ],
      "middleSchools": [
        {
          "servesHome": false,
          "parentRating": 0,
          "distanceInMiles": "0.6",
          "gradeRanges": "7-12",
          "institutionType": "Private",
          "name": "Aviva High School",
          "schoolUrl": "/school/73143/CA/Los-Angeles/Aviva-High-School",
          "searchUrl": "/school/73143/CA/Los-Angeles/Aviva-High-School",
          "greatSchoolOverviewUrl": "https://www.greatschools.org/california/los-angeles/15151-Aviva-High-School/",
          "id": 73143,
          "numberOfStudents": 48,
          "fullAddress": "7120 Franklin Ave, Los Angeles, CA 90046",
          "numReviews": 0,
          "schoolReviews": [],
          "schoolGranularRatings": [],
          "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/los-angeles/15151-Aviva-High-School/#Reviews",
          "elementary": false,
          "middle": true,
          "high": true,
          "lastUpdatedDate": "October 2023",
          "hasMultipleCatchmentAreas": false
        }
      ],
      "highSchools": [
        {
          "servesHome": false,
          "parentRating": 0,
          "distanceInMiles": "0.6",
          "gradeRanges": "7-12",
          "institutionType": "Private",
          "name": "Aviva High School",
          "schoolUrl": "/school/73143/CA/Los-Angeles/Aviva-High-School",
          "searchUrl": "/school/73143/CA/Los-Angeles/Aviva-High-School",
          "greatSchoolOverviewUrl": "https://www.greatschools.org/california/los-angeles/15151-Aviva-High-School/",
          "id": 73143,
          "numberOfStudents": 48,
          "fullAddress": "7120 Franklin Ave, Los Angeles, CA 90046",
          "numReviews": 0,
          "schoolReviews": [],
          "schoolGranularRatings": [],
          "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/los-angeles/15151-Aviva-High-School/#Reviews",
          "elementary": false,
          "middle": true,
          "high": true,
          "lastUpdatedDate": "October 2023",
          "hasMultipleCatchmentAreas": false
        }
      ],
      "servingThisHomeSchools": [
        {
          "servesHome": true,
          "greatSchoolsRating": 6,
          "parentRating": 4,
          "distanceInMiles": "0.1",
          "gradeRanges": "K-5",
          "institutionType": "Public",
          "name": "Gardner Street Elementary School",
          "schoolUrl": "/school/186026/CA/Los-Angeles/Gardner-Street-Elementary-School",
          "searchUrl": "/school/186026/CA/Los-Angeles/Gardner-Street-Elementary-School",
          "greatSchoolOverviewUrl": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/",
          "id": 186026,
          "numberOfStudents": 408,
          "fullAddress": "7450 Hawthorn Ave, Los Angeles, CA 90046",
          "numReviews": 61,
          "studentToTeacherRatio": 25,
          "websiteUrl": "http://www.gardnerstreetschool.org",
          "schoolReviews": [
            {
              "schoolId": 39556114,
              "reviewedBy": "Parent",
              "datePosted": "August 2022",
              "review": "Principal and staff are ignorant. My child learned nothing",
              "reviewerRating": 1,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 5570031,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556113,
              "reviewedBy": "Parent",
              "datePosted": "August 2022",
              "review": "Amazing school! My son went to Gardner street school from pre-K to 5th grade and was the best; great teachers, very involved and committed. Great sense of community.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 5547784,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556112,
              "reviewedBy": "Parent",
              "datePosted": "February 2020",
              "review": "My husband and I assumed that we will have to go Private for our son's Elementary education, because of the urban myth of \"there are no good public schools in Hollywood\". As we were touring all the private schools in the area, we decided to stop by the Open House at our zoned public school, Gardner - just to see for ourselves how much worse it would be. We were shocked to find that we loved everything about Gardner! The way it looks and feels (happy! Filled with art, music and laughter), and mostly its progressive mindset. We were so shaken up that we went back two more times to confirm that our initial impression was not skewed in some way... Our son is now a thriving 2nd grader at Gardner, and we truly do love this school.Gardner Street Elementary is a neighborhood gem with an engaged principal, excellent teachers and helpful staff, and a small but very active parent body that makes things happen. Many students live outside the school zone but they and their parents like Gardner so much that they've chosen to permit in.What makes Gardner so special are its diverse staff and students, family-like atmosphere, and the many \"extras\". Thanks to annual fundraisers coupled with lots of fun events for kids and families throughout the school year, \"non-essential\" classes that are not paid for by LAUSD and not offered in many public schools - such as Visual Art, Music, PE, Yoga, Organic Edible Garden, and (new!) Science Lab - are part of the everyday curriculum and available to every child at Gardner. There are many really great afterschool programs to choose from, and occasional workshops for parents/caregivers. Campus beautification is ongoing.Besides all the fun, there is serious learning going on as well. Learning to truly *understand* and not just memorize to ace tests. So much learning goes on at school that - as supported by countless studies and best-performing schools in Finland - there are no homework requirements for the younger kids.Most importantly, my child has developed love for reading and learning, he is excited about going to school every day, and he feels safe there.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 4418512,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556111,
              "reviewedBy": "Parent",
              "datePosted": "September 2019",
              "review": "We just love Gardner and couldn't have had a better experience. The emphasis on strong values is just amazing and all the kids are so kind! We've found the teachers and admins to be really open to differently-abled kids and their needs. We feel very lucky to have found Gardner!",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 4226639,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556110,
              "reviewedBy": "Parent",
              "datePosted": "September 2019",
              "review": "Originally, we were planning on going to private school then we checked out Gardner and it has turned out awesome.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 4225875,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556109,
              "reviewedBy": "Parent",
              "datePosted": "May 2019",
              "review": "We chose to go to Gardner as a transfer family. When we walked in it felt different than other schools. It was clean, people were friendly and it felt HAPPY! There were happy children around everywhere. It was bright and colorful and pleasant to be in. We have had good teachers, met wonderful parents, and became very involved. The school has a great group of parents running the parent group that raises money all year for the programs that LAUSD doesn't provide. PE, Music, Art, Yoga and Edible Gardening are all only provided because the parents raise the money. And it's not a crazy amount. They aren't hounding you for money. It's usually through fun events like carnivals or dances. They encourage a monthly donation of as little as 5-10 dollars a month and it's totally doable and worth it for what the kids get in the end. Parents don't realize that other public schools don't offer those programs. But Gardner students are thriving thanks to them. We believe in public schools. And LAUSD has a pretty rough reputation, but this is one of those schools that's a diamond in the rough. It is diverse both racially and economically. It's has three main languages spoken among students. It has loving and respectful teachers and administration. Parents that are involved and creating an enriching environment for kids and helping to create a healthier education for them. It's a great size and very inclusive and we are THRILLED to be a part of it.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 4061327,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556108,
              "reviewedBy": "Parent",
              "datePosted": "May 2019",
              "review": "I’m running from this school since we day one. Schools are what you make of it but no ..... unless you pay them for alllll the donations and your attending each bake sale they have an entitled , nasty , belittling attitude. There is no diversity at least with African American or mixed race . They will accent your child but they definitely have the attitude of go else where why are you here.",
              "reviewerRating": 1,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 4056201,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            }
          ],
          "schoolGranularRatings": [
            {
              "schoolId": 285686,
              "dataSourceId": 64,
              "schoolGranularRatingType": "Test Scores",
              "schoolGranularRatingValue": "8",
              "schoolGranularRatingCopyText": "State test results at this school are above the state average, so students are likely performing at or above grade level.\n\nBecause state averages can be low, some students at this school may still not be performing at grade level.\n",
              "schoolGranularRatingsDescription": "The Test Score Rating examines how students at this school performed on standardized tests compared with other schools in the state. The Test Rating was created using 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2022 California Science Test data from California Department of Education."
            },
            {
              "schoolId": 285685,
              "dataSourceId": 64,
              "schoolGranularRatingType": "Equity",
              "schoolGranularRatingValue": "5",
              "schoolGranularRatingCopyText": "Underserved students at this school are performing about as well as other students in the state, but this school may still have achievement gaps between different student groups.",
              "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
            },
            {
              "schoolId": 285687,
              "dataSourceId": 64,
              "schoolGranularRatingType": "Academic Progress",
              "schoolGranularRatingValue": "6",
              "schoolGranularRatingCopyText": "Students at this school are making average academic progress from one grade to the next compared to similar students in the state.",
              "schoolGranularRatingsDescription": "The Academic Progress Rating is a growth proxy rating based on a model using unmatched cohorts, or school-level data instead of student-level data. The Academic Progress Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2019 demographic data from California Department of Education."
            }
          ],
          "schoolDistrict": {
            "id": 9940,
            "districtName": "Los Angeles Unified School District",
            "address": "333 South Beaudry Avenue",
            "city": "Los Angeles",
            "stateCode": "CA",
            "zip": "90017",
            "latitude": 34.056198,
            "longitude": -118.257172,
            "websiteUrl": "http://www.lausd.net",
            "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
          },
          "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/#Reviews",
          "elementary": true,
          "middle": false,
          "high": false,
          "lastUpdatedDate": "October 2023",
          "hasMultipleCatchmentAreas": false
        }
      ],
      "districtsServingThisHome": [
        {
          "elementaryChoiceSchoolsInDistrict": [
            {
              "servesHome": false,
              "greatSchoolsRating": 5,
              "parentRating": 3,
              "distanceInMiles": "5.1",
              "gradeRanges": "1-5",
              "institutionType": "Public",
              "name": "Mid-City's Prescott School of Enriched Sciences",
              "schoolUrl": "/school/24265/CA/Los-Angeles/Mid-City-s-Prescott-School-of-Enriched-Sciences",
              "searchUrl": "/school/24265/CA/Los-Angeles/Mid-City-s-Prescott-School-of-Enriched-Sciences",
              "greatSchoolOverviewUrl": "https://www.greatschools.org/california/los-angeles/10953-Mid-Citys-Prescott-School-Of-Enriched-Sciences/",
              "id": 24265,
              "numberOfStudents": 241,
              "fullAddress": "3150 W Adams Blvd, Los Angeles, CA 90018",
              "isChoice": true,
              "numReviews": 28,
              "studentToTeacherRatio": 24,
              "websiteUrl": "https://www.mcpmagnetschool.org/",
              "schoolReviews": [
                {
                  "schoolId": 39595916,
                  "reviewedBy": "Parent",
                  "datePosted": "August 2023",
                  "review": "We transferred mid-semester in first grade and are so happy at this school! Incredible first and second grade teachers (we don’t know the others yet) and well run. Friendly, warm, helpful staff and excellent principal. We already feel part of the community. This is a hidden gem in LAUSD - a little engine that could. Small community oriented school where everyone is doing their very best!",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610953,
                  "maponicsId": 6050019,
                  "url": "https://www.greatschools.org/california/los-angeles/10953-Mid-Citys-Prescott-School-Of-Enriched-Sciences/"
                },
                {
                  "schoolId": 39595915,
                  "reviewedBy": "Parent",
                  "datePosted": "January 2023",
                  "review": "My daughter transferred to this school, and her experience has been amazing. The teachers and other staff members are very welcoming.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610953,
                  "maponicsId": 5752332,
                  "url": "https://www.greatschools.org/california/los-angeles/10953-Mid-Citys-Prescott-School-Of-Enriched-Sciences/"
                },
                {
                  "schoolId": 39595914,
                  "reviewedBy": "Parent",
                  "datePosted": "January 2023",
                  "review": "My daughter has been at Mid-City’s Prescott for a semester, and she has completely blossomed. She is a more confident student overall. Coming from a very small school preschool-kinder, I was worried about her transition to a bigger school, but this school is more like a family than anything. All of the teachers are so nurturing as well as the parents. If you are considering sending your child here for first grade, I would recommend it. The first-grade teacher is AMAZING and always willing to do anything to assist the students' growth.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610953,
                  "maponicsId": 5752320,
                  "url": "https://www.greatschools.org/california/los-angeles/10953-Mid-Citys-Prescott-School-Of-Enriched-Sciences/"
                },
                {
                  "schoolId": 39595913,
                  "reviewedBy": "Parent",
                  "datePosted": "May 2022",
                  "review": "This little school is a hidden gem in the LAUSD system. It's a small school and my son is completing his first year there as a first grader. I think that every teacher and administrator in the school knows his name and he know most of their's as well. He has friends from almost every grade and feels very at home there. It is a warm and accepting community that I feel so happy to drop him off at everyday. The campus is beautiful, lots of green space and gardens on a hill with views of the city. LAUSD can be an intimidatingly huge bureaucracy, but MidCity's Prescott feels like a small-town community school. So grateful to have found it.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610953,
                  "maponicsId": 5413198,
                  "url": "https://www.greatschools.org/california/los-angeles/10953-Mid-Citys-Prescott-School-Of-Enriched-Sciences/"
                },
                {
                  "schoolId": 39595912,
                  "reviewedBy": "Parent",
                  "datePosted": "April 2022",
                  "review": "Words can't express how happy we are with Mid-City Prescott school. Our twins joined in 4th grade, and we were apprehensive about switching schools at a later point in elementary school and then it was the year of distance learning, so we didn't get the full flavor of the school till this year (in 5th) grade. Dr. Manueal and Ms. Larry are some of the most dedicated teachers that we have ever encountered. From admin down to support staff at school we have found such devotion, above-and-beyond attitude that has helped our boys not only transition well but has helped them to thrive. The school is both academically on top of things and helps support the students with a social emotional attitude which is so important. We definitely give the school 5 out of 5 schools and we are so happy to be part of the Mid-City Prescott school!",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610953,
                  "maponicsId": 5390125,
                  "url": "https://www.greatschools.org/california/los-angeles/10953-Mid-Citys-Prescott-School-Of-Enriched-Sciences/"
                },
                {
                  "schoolId": 39595911,
                  "reviewedBy": "Parent",
                  "datePosted": "April 2022",
                  "review": "Community, community, community! This is little school with a loving environment among the staff, students, and parents. Our son came from a school twice the size, and once he got here, the size of the campus and classrooms allowed us to feel he got plenty of attention from his teacher, staff, and principal. We also love that the campus is full of diversity. Teachers and the principal come from various backgrounds. We love to see our son excited to go to school every day.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610953,
                  "maponicsId": 5387154,
                  "url": "https://www.greatschools.org/california/los-angeles/10953-Mid-Citys-Prescott-School-Of-Enriched-Sciences/"
                },
                {
                  "schoolId": 39595910,
                  "reviewedBy": "Parent",
                  "datePosted": "March 2022",
                  "review": "From the moment we stepped on campus we felt welcome at MCP. The pandemic has added extra to everyone's plates, yet everyone still leads with grace and positivity. I do wish there was a little more lead time on communications, but messages always get to us in time. The supportive staff and teachers have made it so that I can see my child to thriving in this learning space. It is clear to me that my child's teacher wants him to be the best version of himself academically and emotionally and to me, that matters most.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610953,
                  "maponicsId": 5325648,
                  "url": "https://www.greatschools.org/california/los-angeles/10953-Mid-Citys-Prescott-School-Of-Enriched-Sciences/"
                }
              ],
              "schoolGranularRatings": [
                {
                  "schoolId": 291993,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Equity",
                  "schoolGranularRatingValue": "4",
                  "schoolGranularRatingCopyText": "Underserved students at this school may be falling behind other students in the state, and this school may have achievement gaps between different student groups.",
                  "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 291991,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Academic Progress",
                  "schoolGranularRatingValue": "5",
                  "schoolGranularRatingCopyText": "Students at this school are making average academic progress from one grade to the next compared to similar students in the state.",
                  "schoolGranularRatingsDescription": "The Academic Progress Rating is a growth proxy rating based on a model using unmatched cohorts, or school-level data instead of student-level data. The Academic Progress Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 291992,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Test Scores",
                  "schoolGranularRatingValue": "6",
                  "schoolGranularRatingCopyText": "State test results at this school are about the same as the state average, so students are likely performing at grade level.\n\nBecause state averages can be low, some students at this school may still not be performing at grade level.\n",
                  "schoolGranularRatingsDescription": "The Test Score Rating examines how students at this school performed on standardized tests compared with other schools in the state. The Test Rating was created using 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2022 California Science Test data from California Department of Education."
                }
              ],
              "schoolDistrict": {
                "id": 9940,
                "districtName": "Los Angeles Unified School District",
                "address": "333 South Beaudry Avenue",
                "city": "Los Angeles",
                "stateCode": "CA",
                "zip": "90017",
                "latitude": 34.056198,
                "longitude": -118.257172,
                "websiteUrl": "http://www.lausd.net",
                "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
              },
              "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/los-angeles/10953-Mid-Citys-Prescott-School-Of-Enriched-Sciences/#Reviews",
              "elementary": true,
              "middle": false,
              "high": false,
              "lastUpdatedDate": "November 2023",
              "hasMultipleCatchmentAreas": false
            },
            {
              "servesHome": false,
              "greatSchoolsRating": 8,
              "parentRating": 5,
              "distanceInMiles": "8.1",
              "gradeRanges": "K-5",
              "institutionType": "Charter",
              "name": "Para Los Ninos Charter School",
              "schoolUrl": "/school/24459/CA/Los-Angeles/Para-Los-Ninos-Charter-School",
              "searchUrl": "/school/24459/CA/Los-Angeles/Para-Los-Ninos-Charter-School",
              "greatSchoolOverviewUrl": "https://www.greatschools.org/california/los-angeles/12162-Para-Los-Ninos-Charter-School/",
              "id": 24459,
              "numberOfStudents": 365,
              "fullAddress": "1617 E 7th St, Los Angeles, CA 90021",
              "isChoice": true,
              "numReviews": 7,
              "studentToTeacherRatio": 22,
              "websiteUrl": "http://paralosninos.org",
              "schoolReviews": [
                {
                  "schoolId": 39599755,
                  "reviewedBy": "Parent",
                  "datePosted": "December 2018",
                  "review": "As a current parent, I couldn’t be more happy with PLN. This organization focuses on the child as a whole. Their hands on science approach keep students engage and makes them enthusiastic about science",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 612162,
                  "maponicsId": 3780770,
                  "url": "https://www.greatschools.org/california/los-angeles/12162-Para-Los-Ninos-Charter-School/"
                },
                {
                  "schoolId": 5517439,
                  "reviewedBy": "Parent",
                  "datePosted": "April 2014",
                  "review": "PARA LOS NINOS the best school i personally used public school all my life since i found PLN pre-k i love there methods, the teachers are great feels like home never going public for my kids PLN 4 EVER.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 612162,
                  "maponicsId": 1271645,
                  "url": "https://www.greatschools.org/california/los-angeles/12162-Para-Los-Ninos-Charter-School/"
                },
                {
                  "schoolId": 8625886,
                  "reviewedBy": "Parent",
                  "datePosted": "April 2014",
                  "review": "I love the way teachers and principal takes education at the high level My son is so happy to be at PLN charter middle school They always taking the time to meet with parents and students Awesome school awesome teachers!!",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 612162,
                  "maponicsId": 1142579,
                  "url": "https://www.greatschools.org/california/los-angeles/12162-Para-Los-Ninos-Charter-School/"
                },
                {
                  "schoolId": 5517440,
                  "reviewedBy": "Parent",
                  "datePosted": "November 2013",
                  "review": "PARA LOS NINOS CHARTER SCHOOL....IS A FINE SCHOOL DEDICATED TO THE KIDS.....STARTING FROM TEACHERS SCHOOL STAFF and PARENTS",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 612162,
                  "maponicsId": 1409426,
                  "url": "https://www.greatschools.org/california/los-angeles/12162-Para-Los-Ninos-Charter-School/"
                },
                {
                  "schoolId": 14565856,
                  "reviewedBy": "Parent",
                  "datePosted": "February 2011",
                  "review": "PLN is under new administration and is taking a turn to become a high performing school. I am committee to assist the staff to link the staff and parent gap. Is time we take ownership for our school and strength it values and potential. I want a culture of proactive parenting and no excuses at this school. Cause I am not waiting for superman. I want super mom/dad to emerge and take action.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 612162,
                  "maponicsId": 1062246,
                  "url": "https://www.greatschools.org/california/los-angeles/12162-Para-Los-Ninos-Charter-School/"
                },
                {
                  "schoolId": 8625884,
                  "reviewedBy": "Parent",
                  "datePosted": "October 2010",
                  "review": "This is a great school, in may ways better than a private school. All of the staff @ PLN is open and friendly to parents, they care about the children's well being as well as their learning. they are teaching my Kindergarden how to love to learn and that is a very important lesson. He is excited to go to school everyday. Thank you PLN.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 612162,
                  "maponicsId": 1020371,
                  "url": "https://www.greatschools.org/california/los-angeles/12162-Para-Los-Ninos-Charter-School/"
                },
                {
                  "schoolId": 8625883,
                  "reviewedBy": "Parent",
                  "datePosted": "May 2010",
                  "review": "The teachers and staff provide a wonderful atmosphere for learning. Kids are reminded of the important values of life everyday, which is important nowadays with all the stress and tension people are facing in this tough economy!!",
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 612162,
                  "maponicsId": 971219,
                  "url": "https://www.greatschools.org/california/los-angeles/12162-Para-Los-Ninos-Charter-School/"
                }
              ],
              "schoolGranularRatings": [
                {
                  "schoolId": 293032,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Equity",
                  "schoolGranularRatingValue": "6",
                  "schoolGranularRatingCopyText": "Underserved students at this school are performing about as well as other students in the state, but this school may still have achievement gaps between different student groups.",
                  "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 293035,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Test Scores",
                  "schoolGranularRatingValue": "6",
                  "schoolGranularRatingCopyText": "State test results at this school are about the same as the state average, so students are likely performing at grade level.\n\nBecause state averages can be low, some students at this school may still not be performing at grade level.\n",
                  "schoolGranularRatingsDescription": "The Test Score Rating examines how students at this school performed on standardized tests compared with other schools in the state. The Test Rating was created using 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2022 California Science Test data from California Department of Education."
                },
                {
                  "schoolId": 293033,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Academic Progress",
                  "schoolGranularRatingValue": "10",
                  "schoolGranularRatingCopyText": "Students at this school are making more academic progress from one grade to the next compared to similar students in the state.",
                  "schoolGranularRatingsDescription": "The Academic Progress Rating is a growth proxy rating based on a model using unmatched cohorts, or school-level data instead of student-level data. The Academic Progress Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2019 demographic data from California Department of Education."
                }
              ],
              "schoolDistrict": {
                "id": 9940,
                "districtName": "Los Angeles Unified School District",
                "address": "333 South Beaudry Avenue",
                "city": "Los Angeles",
                "stateCode": "CA",
                "zip": "90017",
                "latitude": 34.056198,
                "longitude": -118.257172,
                "websiteUrl": "http://www.lausd.net",
                "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
              },
              "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/los-angeles/12162-Para-Los-Ninos-Charter-School/#Reviews",
              "elementary": true,
              "middle": false,
              "high": false,
              "lastUpdatedDate": "November 2023",
              "hasMultipleCatchmentAreas": false
            },
            {
              "servesHome": false,
              "greatSchoolsRating": 6,
              "parentRating": 4,
              "distanceInMiles": "9.2",
              "gradeRanges": "K-5",
              "institutionType": "Charter",
              "name": "Open Charter Magnet School",
              "schoolUrl": "/school/22830/CA/Los-Angeles/Open-Charter-Magnet-School",
              "searchUrl": "/school/22830/CA/Los-Angeles/Open-Charter-Magnet-School",
              "greatSchoolOverviewUrl": "https://www.greatschools.org/california/los-angeles/2301-Open-Charter-Magnet-School/",
              "id": 22830,
              "numberOfStudents": 405,
              "fullAddress": "5540 W 77th St, Los Angeles, CA 90045",
              "isChoice": true,
              "numReviews": 33,
              "studentToTeacherRatio": 25,
              "websiteUrl": "http://opencharter.org",
              "schoolReviews": [
                {
                  "schoolId": 39557056,
                  "reviewedBy": "Parent",
                  "datePosted": "August 2023",
                  "review": "Open Charter is a great school with wonderful teachers and staff.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602301,
                  "maponicsId": 6064767,
                  "url": "https://www.greatschools.org/california/los-angeles/2301-Open-Charter-Magnet-School/"
                },
                {
                  "schoolId": 39557055,
                  "reviewedBy": "Parent",
                  "datePosted": "May 2021",
                  "review": "Amazing all around. We love everything about it!",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602301,
                  "maponicsId": 4982028,
                  "url": "https://www.greatschools.org/california/los-angeles/2301-Open-Charter-Magnet-School/"
                },
                {
                  "schoolId": 39557054,
                  "reviewedBy": "Parent",
                  "datePosted": "September 2019",
                  "review": "Excellent. The school is very diverse and there are high levels of parent involvement. Thanks to great leadership and tremendous parent support, the school runs well and it's generally a happy place to be. My children are happy and advancing well academically.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602301,
                  "maponicsId": 4204385,
                  "url": "https://www.greatschools.org/california/los-angeles/2301-Open-Charter-Magnet-School/"
                },
                {
                  "schoolId": 39557053,
                  "reviewedBy": "Parent",
                  "datePosted": "August 2019",
                  "review": "If you \"win the lottery\" and get in, accept the spot. But...be vigilant about your child's education.This school has great kids, families, as well as arts & music programs. Unfortunately, in my experience, around 3rd grade with the large class sizes, kids start slipping through the cracks with basic language arts & math fundamentals. There's no schoolwide communication system that allows teachers to let parent know what their kids are working on, how they're doing and things that need to be reinforced. Some clusters are better than others with newsletters or apps about what is happening in class. Twice a year parent/student/teacher conferences are quick & generally, always positive because the child is present. As a parent, you will not get \"flagged\" about academic issues unless you are proactive. 4th and 5th grade cluster class size was especially concerning - 60 kids, 2 teachers and no dedicated aide.",
                  "reviewerRating": 3,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602301,
                  "maponicsId": 4157398,
                  "url": "https://www.greatschools.org/california/los-angeles/2301-Open-Charter-Magnet-School/"
                },
                {
                  "schoolId": 39557052,
                  "reviewedBy": "Parent",
                  "datePosted": "August 2019",
                  "review": "Amazing. It's like I won the lottery.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602301,
                  "maponicsId": 4145752,
                  "url": "https://www.greatschools.org/california/los-angeles/2301-Open-Charter-Magnet-School/"
                },
                {
                  "schoolId": 39557051,
                  "reviewedBy": "Parent",
                  "datePosted": "August 2019",
                  "review": "My daughter just graduated from Open. She started in 1st grade. It's been a terrific experience for her where she has excelled academically, but also Open has helped support her love of learning and freedom to express herself and her feelings. She has made strong friendships too. The teachers are exceptional. They truly care about their students and go above and beyond.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602301,
                  "maponicsId": 4145745,
                  "url": "https://www.greatschools.org/california/los-angeles/2301-Open-Charter-Magnet-School/"
                },
                {
                  "schoolId": 39557050,
                  "reviewedBy": "Parent",
                  "datePosted": "June 2019",
                  "review": "This was my first year at Open. I look forward to continuing on until my child graduates.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602301,
                  "maponicsId": 4101092,
                  "url": "https://www.greatschools.org/california/los-angeles/2301-Open-Charter-Magnet-School/"
                }
              ],
              "schoolGranularRatings": [
                {
                  "schoolId": 286954,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Equity",
                  "schoolGranularRatingValue": "8",
                  "schoolGranularRatingCopyText": "Underserved students at this school are performing better than other students in the state, and this school is likely closing achievement gaps between different student groups.",
                  "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 286953,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Academic Progress",
                  "schoolGranularRatingValue": "3",
                  "schoolGranularRatingCopyText": "Students at this school are making less academic progress from one grade to the next compared to similar students in the state.",
                  "schoolGranularRatingsDescription": "The Academic Progress Rating is a growth proxy rating based on a model using unmatched cohorts, or school-level data instead of student-level data. The Academic Progress Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 286951,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Test Scores",
                  "schoolGranularRatingValue": "9",
                  "schoolGranularRatingCopyText": "State test results at this school are above the state average, so students are likely performing at or above grade level.\n\nBecause state averages can be low, some students at this school may still not be performing at grade level.\n",
                  "schoolGranularRatingsDescription": "The Test Score Rating examines how students at this school performed on standardized tests compared with other schools in the state. The Test Rating was created using 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2022 California Science Test data from California Department of Education."
                }
              ],
              "schoolDistrict": {
                "id": 9940,
                "districtName": "Los Angeles Unified School District",
                "address": "333 South Beaudry Avenue",
                "city": "Los Angeles",
                "stateCode": "CA",
                "zip": "90017",
                "latitude": 34.056198,
                "longitude": -118.257172,
                "websiteUrl": "http://www.lausd.net",
                "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
              },
              "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/los-angeles/2301-Open-Charter-Magnet-School/#Reviews",
              "elementary": true,
              "middle": false,
              "high": false,
              "lastUpdatedDate": "November 2023",
              "hasMultipleCatchmentAreas": false
            },
            {
              "servesHome": false,
              "greatSchoolsRating": 8,
              "parentRating": 3,
              "distanceInMiles": "10.6",
              "gradeRanges": "K-12",
              "institutionType": "Public",
              "name": "Lake Balboa College Preparatory Magnet K-12",
              "schoolUrl": "/school/22921/CA/Los-Angeles/Lake-Balboa-College-Preparatory-Magnet-K-12",
              "searchUrl": "/school/22921/CA/Los-Angeles/Lake-Balboa-College-Preparatory-Magnet-K-12",
              "greatSchoolOverviewUrl": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/",
              "id": 22921,
              "numberOfStudents": 582,
              "fullAddress": "6701 Balboa Blvd, Lake Balboa, CA 91406",
              "isChoice": true,
              "numReviews": 29,
              "studentToTeacherRatio": 22,
              "websiteUrl": "https://www.lakebalboacollegeprep.com/",
              "schoolReviews": [
                {
                  "schoolId": 39595953,
                  "reviewedBy": "Parent",
                  "datePosted": "May 2023",
                  "review": "My child has attended this school for quite some time and it just keeps getting worst especially ever since the current principal Mr. Clark took over. If you ever have any safety concerns or want to discuss anything about your child or the teachers with him he cuts you off never has the time to talk to you or he just gives you the run around. Also the teachers just let the students do whatever they miss assignments and they can just do it when ever they want. I agree with previous reviews this school definitely has a problem with bullying and Mr. Clark and staff will not do anything about it. Its nice that is a small school compared to others but you are better off taking your child elsewhere.",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610954,
                  "maponicsId": 5895254,
                  "url": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/"
                },
                {
                  "schoolId": 39595952,
                  "reviewedBy": "Parent",
                  "datePosted": "March 2023",
                  "review": "I am a Student at this school and there are people sneaking out and the teachers don't really care. If students do something wrong, there are no staff at the playground and the second graders already know to many slurs and the highschool kids are teaching them.",
                  "reviewerRating": 2,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610954,
                  "maponicsId": 5814854,
                  "url": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/"
                },
                {
                  "schoolId": 39595951,
                  "reviewedBy": "Parent",
                  "datePosted": "July 2022",
                  "review": "Not a good school at all always assuming things and end up looking dumb. Principle does not do anything about bullying.",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610954,
                  "maponicsId": 5523769,
                  "url": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/"
                },
                {
                  "schoolId": 39595950,
                  "reviewedBy": "Teacher",
                  "datePosted": "January 2022",
                  "review": "I believe this school is amazing and has definitely expanded in the past few years. There are now 6 different AP (college prep) classes: Chemistry, English Literature, English Language, Psychology, U.S. History, and World History. There is also a lovely farm that allows students to learn responsibility and how to care for animals. It is a very unique Kindergarten through 12th grade school that is relatively small, allowing for a feeling of family and individual attention to all students.",
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610954,
                  "maponicsId": 5263100,
                  "url": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/"
                },
                {
                  "schoolId": 39595949,
                  "reviewedBy": "Parent",
                  "datePosted": "October 2018",
                  "review": "I love this school i bonded with the teachers. Yes some teachers are not that good but most of them are effective and care about you.",
                  "reviewerRating": 4,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610954,
                  "maponicsId": 3653886,
                  "url": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/"
                },
                {
                  "schoolId": 39595948,
                  "reviewedBy": "Other",
                  "datePosted": "June 2017",
                  "review": "I have written about this school years before. Now that I have graduated, I can surely say this school was the worst high school experience ever. A majority of the staff do not care about the students and are quite rude to them. A particular coordinator would not even say \"hi\" back when I would say hi to them. Too much drama, too much of adults acting like children, and most teachers don't even teach except for the science teachers and the government teacher, they are great. Please don't take your kids here. I and others really did not have a good experience with these people.",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610954,
                  "maponicsId": 2717034,
                  "url": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/"
                },
                {
                  "schoolId": 39595947,
                  "reviewedBy": "Student",
                  "datePosted": "January 2017",
                  "review": "I had a great experience at this school.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610954,
                  "maponicsId": 2433179,
                  "url": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/"
                }
              ],
              "schoolGranularRatings": [
                {
                  "schoolId": 287261,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Equity",
                  "schoolGranularRatingValue": "7",
                  "schoolGranularRatingCopyText": "Underserved students at this school are performing better than other students in the state, and this school is likely closing achievement gaps between different student groups.",
                  "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 287262,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "College Readiness",
                  "schoolGranularRatingValue": "9",
                  "schoolGranularRatingCopyText": "This school is above the state average in key measures of college and career readiness, such as graduation rates, college entrance exams, and advanced courses.\n\nEven at schools with strong college and career readiness, there may be students who are not getting the opportunities they need to succeed.\n",
                  "schoolGranularRatingsDescription": "The College Readiness Rating uses this high school's graduation rates, college entrance exam participation and performance, or AP, IB, or Dual Enrollment participation to determine how well schools are preparing students for success in college and beyond. The College Readiness Rating was created using the following data from the 2018 Civil Rights Data Collection: percentage of students enrolled in IB, AP or Dual Enrollment classes in grades 9-12, using 2019 SAT percent college ready data from California Department of Education, using 2019 Average ACT score data from California Department of Education, using 2019 ACT percent college ready data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 287263,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Academic Progress",
                  "schoolGranularRatingValue": "7",
                  "schoolGranularRatingCopyText": "Students at this school are making more academic progress from one grade to the next compared to similar students in the state.",
                  "schoolGranularRatingsDescription": "The Academic Progress Rating is a growth proxy rating based on a model using unmatched cohorts, or school-level data instead of student-level data. The Academic Progress Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2019 demographic data from California Department of Education."
                }
              ],
              "schoolDistrict": {
                "id": 9940,
                "districtName": "Los Angeles Unified School District",
                "address": "333 South Beaudry Avenue",
                "city": "Los Angeles",
                "stateCode": "CA",
                "zip": "90017",
                "latitude": 34.056198,
                "longitude": -118.257172,
                "websiteUrl": "http://www.lausd.net",
                "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
              },
              "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/#Reviews",
              "elementary": true,
              "middle": true,
              "high": true,
              "lastUpdatedDate": "November 2023",
              "hasMultipleCatchmentAreas": false
            },
            {
              "servesHome": false,
              "greatSchoolsRating": 8,
              "parentRating": 3,
              "distanceInMiles": "12.1",
              "gradeRanges": "4-12",
              "institutionType": "Public",
              "name": "Sherman Oaks Center For Enriched Studies",
              "schoolUrl": "/school/24145/CA/Los-Angeles/Sherman-Oaks-Center-For-Enriched-Studies",
              "searchUrl": "/school/24145/CA/Los-Angeles/Sherman-Oaks-Center-For-Enriched-Studies",
              "greatSchoolOverviewUrl": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/",
              "id": 24145,
              "numberOfStudents": 2087,
              "fullAddress": "18605 Erwin St, Reseda, CA 91335",
              "isChoice": true,
              "numReviews": 28,
              "studentToTeacherRatio": 30,
              "websiteUrl": "http://www.shermanoaksces.com",
              "schoolReviews": [
                {
                  "schoolId": 39595964,
                  "reviewedBy": "Parent",
                  "datePosted": "May 2023",
                  "review": "this school is literally insane and im not being overdramatic some of the teachers are literally pervs and need to get fired but since they cant find anyone they cant fire them all the students are smelly and I'm not joking they stink and they're so mean and gross people overall this school needs like a wellness check or smth there's only like two good teachers here and they deserve better and so do we thanks don't send ur children here",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610957,
                  "maponicsId": 5927431,
                  "url": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/"
                },
                {
                  "schoolId": 39595963,
                  "reviewedBy": "Student",
                  "datePosted": "January 2021",
                  "review": "I have been attending this school for over six years now and I feel as though it is necessary to write a review. This school is academically standard, but it is promising that most of the students here will get to a university or college of their choice.Homework: Some teachers take it upon themselves to assign endless amounts of homework, giving students barely enough time to get the amount of rest is needed. There are 3-6 classes a day and we are given so much homework from each of them, it’s no wonder why we’re exhausted. Teachers (and staff)In addition, the majority staff and teachers rarely provide any care to problems such as bullying or a problematic teacher/sub assigning the most ridiculous work. Even negative online activity isn’t even being looked upon simply because the staff “isn’t being paid enough” to do anything about it. Now, with online learning, the teachers are assigning work that they don’t even teach. Why is the synchronous class time so long then?LeadershipI don’t have much to say about leadership but there are activities where students and parents can be involved in. The new principal shows promising leadership but we’ll see what happens. CharacterRegarding character, SOCES has a lot of rules but they’re not really enforced so we are mostly free with what we do, kind of like what happens if you were out in town. It develops who we are as people in the world outside of school. Learning DifferencesAlthough we go over learning differences in many classes, there just isn’t enough that teachers do to support all of them. They just want to get over with the classes but occasionally they help. BullyingThe staff “isn’t being paid enough” to give much regard to bullying, especially cyberbullying. In cases that there is bullying, both parties get sent to the counselor and get a measly warning.",
                  "reviewerRating": 3,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610957,
                  "maponicsId": 4807442,
                  "url": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/"
                },
                {
                  "schoolId": 39595962,
                  "reviewedBy": "Parent",
                  "datePosted": "November 2019",
                  "review": "Overall Good - ...The administration need to have better customer services with our kids and parents. The teachers need to not give out excessive homework, classrooms should be formatted smaller on the student to teacher ratio and the school needs a make-over, which is occurring now.",
                  "reviewerRating": 3,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610957,
                  "maponicsId": 4277208,
                  "url": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/"
                },
                {
                  "schoolId": 39595961,
                  "reviewedBy": "Parent",
                  "datePosted": "April 2019",
                  "review": "The school didn't meet our expectations. It seems like the perfect school with high ratings, but there's lots of bullying and the administration buries all their issues. There is always some kind of problem that no one ever fixes. There are some amazing teachers who care about the student. I can't say the same about the other staff.",
                  "reviewerRating": 2,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610957,
                  "maponicsId": 3956937,
                  "url": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/"
                },
                {
                  "schoolId": 39595960,
                  "reviewedBy": "Student",
                  "datePosted": "March 2019",
                  "review": "This school is wonderful in learning and character and I give it 5 stars!!! I am in 5 th grade and my math and science/ health teacher is one of the most amazing, kind, and inspiring educators ever!! Her name is Mrs. Gibson and all the kids in my class love her! We do a lot of fun and educational projects.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610957,
                  "maponicsId": 3924523,
                  "url": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/"
                },
                {
                  "schoolId": 39595959,
                  "reviewedBy": "Parent",
                  "datePosted": "April 2018",
                  "review": "My daughter loves it here. I love it overall. They have great programs and electives. The school's booster club and PTA needs to back off the staff and parents for money though. I get it, the kids need money for field trips. But my kid has been in the school for 3 years and has taken only one trip, so where is the money really going? Aside from that. LOVE THIS SCHOOL!",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610957,
                  "maponicsId": 3330516,
                  "url": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/"
                },
                {
                  "schoolId": 39595958,
                  "reviewedBy": "Parent",
                  "datePosted": "June 2017",
                  "review": "In our experience, it was ok. Its a safe place but its not the 'private' school of the public schools like it once was. The well being of the students does not come first at this school. Its all about what is convenient for the teachers.",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610957,
                  "maponicsId": 2683450,
                  "url": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/"
                }
              ],
              "schoolGranularRatings": [
                {
                  "schoolId": 291359,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Equity",
                  "schoolGranularRatingValue": "7",
                  "schoolGranularRatingCopyText": "Underserved students at this school are performing better than other students in the state, and this school is likely closing achievement gaps between different student groups.",
                  "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 291356,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Academic Progress",
                  "schoolGranularRatingValue": "6",
                  "schoolGranularRatingCopyText": "Students at this school are making average academic progress from one grade to the next compared to similar students in the state.",
                  "schoolGranularRatingsDescription": "The Academic Progress Rating is a growth proxy rating based on a model using unmatched cohorts, or school-level data instead of student-level data. The Academic Progress Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 291357,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "College Readiness",
                  "schoolGranularRatingValue": "10",
                  "schoolGranularRatingCopyText": "This school is above the state average in key measures of college and career readiness, such as graduation rates, college entrance exams, and advanced courses.\n\nEven at schools with strong college and career readiness, there may be students who are not getting the opportunities they need to succeed.\n",
                  "schoolGranularRatingsDescription": "The College Readiness Rating uses this high school's graduation rates, college entrance exam participation and performance, or AP, IB, or Dual Enrollment participation to determine how well schools are preparing students for success in college and beyond. The College Readiness Rating was created using the following data from the 2018 Civil Rights Data Collection: percentage of students enrolled in IB, AP or Dual Enrollment classes in grades 9-12, using 2019 SAT percent college ready data from California Department of Education, using 2019 Average ACT score data from California Department of Education, using 2019 ACT percent college ready data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 291355,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Test Scores",
                  "schoolGranularRatingValue": "8",
                  "schoolGranularRatingCopyText": "State test results at this school are above the state average, so students are likely performing at or above grade level.\n\nBecause state averages can be low, some students at this school may still not be performing at grade level.\n",
                  "schoolGranularRatingsDescription": "The Test Score Rating examines how students at this school performed on standardized tests compared with other schools in the state. The Test Rating was created using 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2022 California Science Test data from California Department of Education."
                }
              ],
              "schoolDistrict": {
                "id": 9940,
                "districtName": "Los Angeles Unified School District",
                "address": "333 South Beaudry Avenue",
                "city": "Los Angeles",
                "stateCode": "CA",
                "zip": "90017",
                "latitude": 34.056198,
                "longitude": -118.257172,
                "websiteUrl": "http://www.lausd.net",
                "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
              },
              "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/#Reviews",
              "elementary": true,
              "middle": true,
              "high": true,
              "lastUpdatedDate": "November 2023",
              "hasMultipleCatchmentAreas": false
            },
            {
              "servesHome": false,
              "greatSchoolsRating": 8,
              "parentRating": 4,
              "distanceInMiles": "16.0",
              "gradeRanges": "K-8",
              "institutionType": "Charter",
              "name": "Our Community Charter School",
              "schoolUrl": "/school/22803/CA/Los-Angeles/Our-Community-Charter-School",
              "searchUrl": "/school/22803/CA/Los-Angeles/Our-Community-Charter-School",
              "greatSchoolOverviewUrl": "https://www.greatschools.org/california/chatsworth/14935-Our-Community-Charter-School/",
              "id": 22803,
              "numberOfStudents": 447,
              "fullAddress": "10045 Jumilla Ave, Chatsworth, CA 91311",
              "isChoice": true,
              "numReviews": 27,
              "studentToTeacherRatio": 18,
              "websiteUrl": "http://ourcommunityschool.org",
              "schoolReviews": [
                {
                  "schoolId": 39608336,
                  "reviewedBy": "Parent",
                  "datePosted": "May 2023",
                  "review": "OCS is over-rated. It does feel safe and secluded, I’ll give them get 5 stars for that. The kids do make good friends and social development is important; but OCS focuses more on a sociological agenda than it does academics. OCS does not try to nurture students who have an aptitude or a desire for learning. Excellence is thwarted. Students don't get homework at this school. As my child got older, I realized the no-homework policy is so teachers don't have to grade school work at the expense of my child’s education. There is no testing for gifted students here. If your child might be gifted do not come here it will become impossible to move into a gifted school from here. OCS does all they can to treat all students equally; there is to be no bright shining stars at OCS, everyone has to be equal. This school is great for special ed students, the school does all they can to elevate special ed students.",
                  "reviewerRating": 3,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 614935,
                  "maponicsId": 5912404,
                  "url": "https://www.greatschools.org/california/chatsworth/14935-Our-Community-Charter-School/"
                },
                {
                  "schoolId": 39608335,
                  "reviewedBy": "Parent",
                  "datePosted": "November 2022",
                  "review": "Amazing school with amazing teachers that go above and beyond.They really care about their students and are not just there for a paycheck. We are very happy seeing how much our child is learning and loving going to school everyday.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 614935,
                  "maponicsId": 5671267,
                  "url": "https://www.greatschools.org/california/chatsworth/14935-Our-Community-Charter-School/"
                },
                {
                  "schoolId": 39608334,
                  "reviewedBy": "Other",
                  "datePosted": "March 2022",
                  "review": "Former employee-I have a disability and asked Vice Principal, Beth, for accommodations that would fallen under the umbrella of the ADA and was denied. She explained that I was clearly using my disability as a crutch (mind you I had never asked for anything and the accommodation I asked for would not have cost a cent) but she felt that I was using my disability as a way to excuse my incompetence. Disabilities are not one size fits all and Our Community needs to do a better job at accommodating employees with disabilities or perhaps read up on the ADA before denying a disabled employee their rights.",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 614935,
                  "maponicsId": 5355784,
                  "url": "https://www.greatschools.org/california/chatsworth/14935-Our-Community-Charter-School/"
                },
                {
                  "schoolId": 39608333,
                  "reviewedBy": "Parent",
                  "datePosted": "November 2021",
                  "review": "This school advertised itself as a school that understands disability and diversity; however, our experience proved otherwise. The teachers did not show any understanding of disability and blamed my child for her difficulties. The BID (the aid’s supervisor) did not have appropriate training to supervise the BII. The BID (supervisor) engaged in a multiple relationship with my child, being her counselor and her BID at the same time, which is unethical. The children were bullying my child and the teachers and administration blamed my child for it. The assistant principal was not responsive to our needs. We had to pull our child out of the school in the middle of the semester because she was suffering there and was not learning anything.",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 614935,
                  "maponicsId": 5178076,
                  "url": "https://www.greatschools.org/california/chatsworth/14935-Our-Community-Charter-School/"
                },
                {
                  "schoolId": 39608332,
                  "reviewedBy": "Parent",
                  "datePosted": "May 2021",
                  "review": "Five years in, we continue to feel so grateful for this school even with some ups and downs over the years. As parents, we appreciate the community so much-- including the staff as well as the other families that we've met there. But much more important, both of our kids love going to school every day, which was not true *at all* when our son started kindergarten at our neighborhood school. We switched as soon as we had the chance, and witnessed first hand the difference that the OCS commitment to social and emotional wellness makes. And we are so grateful for the hard work, the dedication, and the difficult decision-making the administration put into reopening after closing due to COVID. Our kids are thrilled to be back at OCS, and so are we!",
                  "reviewerRating": 4,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 614935,
                  "maponicsId": 4968348,
                  "url": "https://www.greatschools.org/california/chatsworth/14935-Our-Community-Charter-School/"
                },
                {
                  "schoolId": 39608331,
                  "reviewedBy": "Parent",
                  "datePosted": "June 2019",
                  "review": "It used to be a wonderful school, a community of parents and children. But the school leadership has been destroying that community since the current executive director was hired as principal. The education was good, but if you're expecting a progressive education you'll be disappointed. It's a traditional school trying to promote itself as if it's something else.",
                  "reviewerRating": 2,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 614935,
                  "maponicsId": 4082635,
                  "url": "https://www.greatschools.org/california/chatsworth/14935-Our-Community-Charter-School/"
                },
                {
                  "schoolId": 39608330,
                  "reviewedBy": "Parent",
                  "datePosted": "June 2019",
                  "review": "It was beautiful, my children loved it, but sadly is not the same",
                  "reviewerRating": 2,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 614935,
                  "maponicsId": 4070389,
                  "url": "https://www.greatschools.org/california/chatsworth/14935-Our-Community-Charter-School/"
                }
              ],
              "schoolGranularRatings": [
                {
                  "schoolId": 286801,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Test Scores",
                  "schoolGranularRatingValue": "9",
                  "schoolGranularRatingCopyText": "State test results at this school are above the state average, so students are likely performing at or above grade level.\n\nBecause state averages can be low, some students at this school may still not be performing at grade level.\n",
                  "schoolGranularRatingsDescription": "The Test Score Rating examines how students at this school performed on standardized tests compared with other schools in the state. The Test Rating was created using 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2022 California Science Test data from California Department of Education."
                },
                {
                  "schoolId": 286799,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Equity",
                  "schoolGranularRatingValue": "9",
                  "schoolGranularRatingCopyText": "Underserved students at this school are performing better than other students in the state, and this school is likely closing achievement gaps between different student groups.",
                  "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 286800,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Academic Progress",
                  "schoolGranularRatingValue": "6",
                  "schoolGranularRatingCopyText": "Students at this school are making average academic progress from one grade to the next compared to similar students in the state.",
                  "schoolGranularRatingsDescription": "The Academic Progress Rating is a growth proxy rating based on a model using unmatched cohorts, or school-level data instead of student-level data. The Academic Progress Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2019 demographic data from California Department of Education."
                }
              ],
              "schoolDistrict": {
                "id": 9940,
                "districtName": "Los Angeles Unified School District",
                "address": "333 South Beaudry Avenue",
                "city": "Los Angeles",
                "stateCode": "CA",
                "zip": "90017",
                "latitude": 34.056198,
                "longitude": -118.257172,
                "websiteUrl": "http://www.lausd.net",
                "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
              },
              "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/chatsworth/14935-Our-Community-Charter-School/#Reviews",
              "elementary": true,
              "middle": true,
              "high": false,
              "lastUpdatedDate": "November 2023",
              "hasMultipleCatchmentAreas": false
            }
          ],
          "middleChoiceSchoolsInDistrict": [
            {
              "servesHome": false,
              "greatSchoolsRating": 5,
              "parentRating": 4,
              "distanceInMiles": "4.9",
              "gradeRanges": "5-8",
              "institutionType": "Charter",
              "name": "Stella Middle Charter Academy",
              "schoolUrl": "/school/24597/CA/Los-Angeles/Stella-Middle-Charter-Academy",
              "searchUrl": "/school/24597/CA/Los-Angeles/Stella-Middle-Charter-Academy",
              "greatSchoolOverviewUrl": "https://www.greatschools.org/california/los-angeles/12310-Stella-Middle-Charter-Academy/",
              "id": 24597,
              "numberOfStudents": 498,
              "fullAddress": "600 S La Fayette Park Pl, Los Angeles, CA 90057",
              "isChoice": true,
              "numReviews": 22,
              "studentToTeacherRatio": 20,
              "websiteUrl": "http://brightstarschools.org/SMCA",
              "schoolReviews": [
                {
                  "schoolId": 39600360,
                  "reviewedBy": "Parent",
                  "datePosted": "September 2016",
                  "review": "so far this been the best school .im so happy i move my 2 girl.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 612310,
                  "maponicsId": 2282157,
                  "url": "https://www.greatschools.org/california/los-angeles/12310-Stella-Middle-Charter-Academy/"
                },
                {
                  "schoolId": 39600359,
                  "reviewedBy": "Other",
                  "datePosted": "September 2016",
                  "review": "I feel this way because in Stella middle charter academic they help when i need help also i feel safe in this school",
                  "reviewerRating": 4,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 612310,
                  "maponicsId": 2282155,
                  "url": "https://www.greatschools.org/california/los-angeles/12310-Stella-Middle-Charter-Academy/"
                },
                {
                  "schoolId": 39600358,
                  "reviewedBy": "Other",
                  "datePosted": "September 2016",
                  "review": "Its big and i feel safe.I get help on what i need. we do fun activities and learn at the same time.",
                  "reviewerRating": 4,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 612310,
                  "maponicsId": 2282151,
                  "url": "https://www.greatschools.org/california/los-angeles/12310-Stella-Middle-Charter-Academy/"
                },
                {
                  "schoolId": 39600361,
                  "reviewedBy": "Student",
                  "datePosted": "September 2016",
                  "review": "I like this school because they teach me a lot of things. They push me to do better.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 612310,
                  "maponicsId": 2282168,
                  "url": "https://www.greatschools.org/california/los-angeles/12310-Stella-Middle-Charter-Academy/"
                },
                {
                  "schoolId": 39600357,
                  "reviewedBy": "Parent",
                  "datePosted": "September 2016",
                  "review": "por qeu mi hija le gusta yle a ayudado mucho superarse pienso que tiene muchas oportunidades para estudiar loque se proponga",
                  "reviewerRating": 4,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 612310,
                  "maponicsId": 2280697,
                  "url": "https://www.greatschools.org/california/los-angeles/12310-Stella-Middle-Charter-Academy/"
                },
                {
                  "schoolId": 39600356,
                  "reviewedBy": "Parent",
                  "datePosted": "September 2016",
                  "review": "This is my first year at the school. So far my child is learning well. He is happy at this school.",
                  "reviewerRating": 4,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 612310,
                  "maponicsId": 2280475,
                  "url": "https://www.greatschools.org/california/los-angeles/12310-Stella-Middle-Charter-Academy/"
                },
                {
                  "schoolId": 39600355,
                  "reviewedBy": "Parent",
                  "datePosted": "September 2016",
                  "review": "This school is one of the best. My kids have been able to learn so much. They were behind with their reading before joining Stella Middle Charter Academy. Now my kids are doing so well.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 612310,
                  "maponicsId": 2280434,
                  "url": "https://www.greatschools.org/california/los-angeles/12310-Stella-Middle-Charter-Academy/"
                }
              ],
              "schoolGranularRatings": [
                {
                  "schoolId": 293801,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Test Scores",
                  "schoolGranularRatingValue": "4",
                  "schoolGranularRatingCopyText": "State test results at this school are below the state average, so students are likely performing below grade level.",
                  "schoolGranularRatingsDescription": "The Test Score Rating examines how students at this school performed on standardized tests compared with other schools in the state. The Test Rating was created using 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2022 California Science Test data from California Department of Education."
                },
                {
                  "schoolId": 293798,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Academic Progress",
                  "schoolGranularRatingValue": "6",
                  "schoolGranularRatingCopyText": "Students at this school are making average academic progress from one grade to the next compared to similar students in the state.",
                  "schoolGranularRatingsDescription": "The Academic Progress Rating is a growth proxy rating based on a model using unmatched cohorts, or school-level data instead of student-level data. The Academic Progress Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 293799,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Equity",
                  "schoolGranularRatingValue": "5",
                  "schoolGranularRatingCopyText": "Underserved students at this school are performing about as well as other students in the state, but this school may still have achievement gaps between different student groups.",
                  "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                }
              ],
              "schoolDistrict": {
                "id": 9940,
                "districtName": "Los Angeles Unified School District",
                "address": "333 South Beaudry Avenue",
                "city": "Los Angeles",
                "stateCode": "CA",
                "zip": "90017",
                "latitude": 34.056198,
                "longitude": -118.257172,
                "websiteUrl": "http://www.lausd.net",
                "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
              },
              "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/los-angeles/12310-Stella-Middle-Charter-Academy/#Reviews",
              "elementary": true,
              "middle": true,
              "high": false,
              "lastUpdatedDate": "November 2023",
              "hasMultipleCatchmentAreas": false
            },
            {
              "servesHome": false,
              "greatSchoolsRating": 8,
              "parentRating": 3,
              "distanceInMiles": "10.6",
              "gradeRanges": "K-12",
              "institutionType": "Public",
              "name": "Lake Balboa College Preparatory Magnet K-12",
              "schoolUrl": "/school/22921/CA/Los-Angeles/Lake-Balboa-College-Preparatory-Magnet-K-12",
              "searchUrl": "/school/22921/CA/Los-Angeles/Lake-Balboa-College-Preparatory-Magnet-K-12",
              "greatSchoolOverviewUrl": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/",
              "id": 22921,
              "numberOfStudents": 582,
              "fullAddress": "6701 Balboa Blvd, Lake Balboa, CA 91406",
              "isChoice": true,
              "numReviews": 29,
              "studentToTeacherRatio": 22,
              "websiteUrl": "https://www.lakebalboacollegeprep.com/",
              "schoolReviews": [
                {
                  "schoolId": 39595953,
                  "reviewedBy": "Parent",
                  "datePosted": "May 2023",
                  "review": "My child has attended this school for quite some time and it just keeps getting worst especially ever since the current principal Mr. Clark took over. If you ever have any safety concerns or want to discuss anything about your child or the teachers with him he cuts you off never has the time to talk to you or he just gives you the run around. Also the teachers just let the students do whatever they miss assignments and they can just do it when ever they want. I agree with previous reviews this school definitely has a problem with bullying and Mr. Clark and staff will not do anything about it. Its nice that is a small school compared to others but you are better off taking your child elsewhere.",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610954,
                  "maponicsId": 5895254,
                  "url": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/"
                },
                {
                  "schoolId": 39595952,
                  "reviewedBy": "Parent",
                  "datePosted": "March 2023",
                  "review": "I am a Student at this school and there are people sneaking out and the teachers don't really care. If students do something wrong, there are no staff at the playground and the second graders already know to many slurs and the highschool kids are teaching them.",
                  "reviewerRating": 2,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610954,
                  "maponicsId": 5814854,
                  "url": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/"
                },
                {
                  "schoolId": 39595951,
                  "reviewedBy": "Parent",
                  "datePosted": "July 2022",
                  "review": "Not a good school at all always assuming things and end up looking dumb. Principle does not do anything about bullying.",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610954,
                  "maponicsId": 5523769,
                  "url": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/"
                },
                {
                  "schoolId": 39595950,
                  "reviewedBy": "Teacher",
                  "datePosted": "January 2022",
                  "review": "I believe this school is amazing and has definitely expanded in the past few years. There are now 6 different AP (college prep) classes: Chemistry, English Literature, English Language, Psychology, U.S. History, and World History. There is also a lovely farm that allows students to learn responsibility and how to care for animals. It is a very unique Kindergarten through 12th grade school that is relatively small, allowing for a feeling of family and individual attention to all students.",
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610954,
                  "maponicsId": 5263100,
                  "url": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/"
                },
                {
                  "schoolId": 39595949,
                  "reviewedBy": "Parent",
                  "datePosted": "October 2018",
                  "review": "I love this school i bonded with the teachers. Yes some teachers are not that good but most of them are effective and care about you.",
                  "reviewerRating": 4,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610954,
                  "maponicsId": 3653886,
                  "url": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/"
                },
                {
                  "schoolId": 39595948,
                  "reviewedBy": "Other",
                  "datePosted": "June 2017",
                  "review": "I have written about this school years before. Now that I have graduated, I can surely say this school was the worst high school experience ever. A majority of the staff do not care about the students and are quite rude to them. A particular coordinator would not even say \"hi\" back when I would say hi to them. Too much drama, too much of adults acting like children, and most teachers don't even teach except for the science teachers and the government teacher, they are great. Please don't take your kids here. I and others really did not have a good experience with these people.",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610954,
                  "maponicsId": 2717034,
                  "url": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/"
                },
                {
                  "schoolId": 39595947,
                  "reviewedBy": "Student",
                  "datePosted": "January 2017",
                  "review": "I had a great experience at this school.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610954,
                  "maponicsId": 2433179,
                  "url": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/"
                }
              ],
              "schoolGranularRatings": [
                {
                  "schoolId": 287261,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Equity",
                  "schoolGranularRatingValue": "7",
                  "schoolGranularRatingCopyText": "Underserved students at this school are performing better than other students in the state, and this school is likely closing achievement gaps between different student groups.",
                  "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 287262,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "College Readiness",
                  "schoolGranularRatingValue": "9",
                  "schoolGranularRatingCopyText": "This school is above the state average in key measures of college and career readiness, such as graduation rates, college entrance exams, and advanced courses.\n\nEven at schools with strong college and career readiness, there may be students who are not getting the opportunities they need to succeed.\n",
                  "schoolGranularRatingsDescription": "The College Readiness Rating uses this high school's graduation rates, college entrance exam participation and performance, or AP, IB, or Dual Enrollment participation to determine how well schools are preparing students for success in college and beyond. The College Readiness Rating was created using the following data from the 2018 Civil Rights Data Collection: percentage of students enrolled in IB, AP or Dual Enrollment classes in grades 9-12, using 2019 SAT percent college ready data from California Department of Education, using 2019 Average ACT score data from California Department of Education, using 2019 ACT percent college ready data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 287263,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Academic Progress",
                  "schoolGranularRatingValue": "7",
                  "schoolGranularRatingCopyText": "Students at this school are making more academic progress from one grade to the next compared to similar students in the state.",
                  "schoolGranularRatingsDescription": "The Academic Progress Rating is a growth proxy rating based on a model using unmatched cohorts, or school-level data instead of student-level data. The Academic Progress Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2019 demographic data from California Department of Education."
                }
              ],
              "schoolDistrict": {
                "id": 9940,
                "districtName": "Los Angeles Unified School District",
                "address": "333 South Beaudry Avenue",
                "city": "Los Angeles",
                "stateCode": "CA",
                "zip": "90017",
                "latitude": 34.056198,
                "longitude": -118.257172,
                "websiteUrl": "http://www.lausd.net",
                "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
              },
              "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/#Reviews",
              "elementary": true,
              "middle": true,
              "high": true,
              "lastUpdatedDate": "November 2023",
              "hasMultipleCatchmentAreas": false
            },
            {
              "servesHome": false,
              "greatSchoolsRating": 8,
              "parentRating": 3,
              "distanceInMiles": "12.1",
              "gradeRanges": "4-12",
              "institutionType": "Public",
              "name": "Sherman Oaks Center For Enriched Studies",
              "schoolUrl": "/school/24145/CA/Los-Angeles/Sherman-Oaks-Center-For-Enriched-Studies",
              "searchUrl": "/school/24145/CA/Los-Angeles/Sherman-Oaks-Center-For-Enriched-Studies",
              "greatSchoolOverviewUrl": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/",
              "id": 24145,
              "numberOfStudents": 2087,
              "fullAddress": "18605 Erwin St, Reseda, CA 91335",
              "isChoice": true,
              "numReviews": 28,
              "studentToTeacherRatio": 30,
              "websiteUrl": "http://www.shermanoaksces.com",
              "schoolReviews": [
                {
                  "schoolId": 39595964,
                  "reviewedBy": "Parent",
                  "datePosted": "May 2023",
                  "review": "this school is literally insane and im not being overdramatic some of the teachers are literally pervs and need to get fired but since they cant find anyone they cant fire them all the students are smelly and I'm not joking they stink and they're so mean and gross people overall this school needs like a wellness check or smth there's only like two good teachers here and they deserve better and so do we thanks don't send ur children here",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610957,
                  "maponicsId": 5927431,
                  "url": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/"
                },
                {
                  "schoolId": 39595963,
                  "reviewedBy": "Student",
                  "datePosted": "January 2021",
                  "review": "I have been attending this school for over six years now and I feel as though it is necessary to write a review. This school is academically standard, but it is promising that most of the students here will get to a university or college of their choice.Homework: Some teachers take it upon themselves to assign endless amounts of homework, giving students barely enough time to get the amount of rest is needed. There are 3-6 classes a day and we are given so much homework from each of them, it’s no wonder why we’re exhausted. Teachers (and staff)In addition, the majority staff and teachers rarely provide any care to problems such as bullying or a problematic teacher/sub assigning the most ridiculous work. Even negative online activity isn’t even being looked upon simply because the staff “isn’t being paid enough” to do anything about it. Now, with online learning, the teachers are assigning work that they don’t even teach. Why is the synchronous class time so long then?LeadershipI don’t have much to say about leadership but there are activities where students and parents can be involved in. The new principal shows promising leadership but we’ll see what happens. CharacterRegarding character, SOCES has a lot of rules but they’re not really enforced so we are mostly free with what we do, kind of like what happens if you were out in town. It develops who we are as people in the world outside of school. Learning DifferencesAlthough we go over learning differences in many classes, there just isn’t enough that teachers do to support all of them. They just want to get over with the classes but occasionally they help. BullyingThe staff “isn’t being paid enough” to give much regard to bullying, especially cyberbullying. In cases that there is bullying, both parties get sent to the counselor and get a measly warning.",
                  "reviewerRating": 3,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610957,
                  "maponicsId": 4807442,
                  "url": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/"
                },
                {
                  "schoolId": 39595962,
                  "reviewedBy": "Parent",
                  "datePosted": "November 2019",
                  "review": "Overall Good - ...The administration need to have better customer services with our kids and parents. The teachers need to not give out excessive homework, classrooms should be formatted smaller on the student to teacher ratio and the school needs a make-over, which is occurring now.",
                  "reviewerRating": 3,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610957,
                  "maponicsId": 4277208,
                  "url": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/"
                },
                {
                  "schoolId": 39595961,
                  "reviewedBy": "Parent",
                  "datePosted": "April 2019",
                  "review": "The school didn't meet our expectations. It seems like the perfect school with high ratings, but there's lots of bullying and the administration buries all their issues. There is always some kind of problem that no one ever fixes. There are some amazing teachers who care about the student. I can't say the same about the other staff.",
                  "reviewerRating": 2,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610957,
                  "maponicsId": 3956937,
                  "url": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/"
                },
                {
                  "schoolId": 39595960,
                  "reviewedBy": "Student",
                  "datePosted": "March 2019",
                  "review": "This school is wonderful in learning and character and I give it 5 stars!!! I am in 5 th grade and my math and science/ health teacher is one of the most amazing, kind, and inspiring educators ever!! Her name is Mrs. Gibson and all the kids in my class love her! We do a lot of fun and educational projects.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610957,
                  "maponicsId": 3924523,
                  "url": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/"
                },
                {
                  "schoolId": 39595959,
                  "reviewedBy": "Parent",
                  "datePosted": "April 2018",
                  "review": "My daughter loves it here. I love it overall. They have great programs and electives. The school's booster club and PTA needs to back off the staff and parents for money though. I get it, the kids need money for field trips. But my kid has been in the school for 3 years and has taken only one trip, so where is the money really going? Aside from that. LOVE THIS SCHOOL!",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610957,
                  "maponicsId": 3330516,
                  "url": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/"
                },
                {
                  "schoolId": 39595958,
                  "reviewedBy": "Parent",
                  "datePosted": "June 2017",
                  "review": "In our experience, it was ok. Its a safe place but its not the 'private' school of the public schools like it once was. The well being of the students does not come first at this school. Its all about what is convenient for the teachers.",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610957,
                  "maponicsId": 2683450,
                  "url": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/"
                }
              ],
              "schoolGranularRatings": [
                {
                  "schoolId": 291359,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Equity",
                  "schoolGranularRatingValue": "7",
                  "schoolGranularRatingCopyText": "Underserved students at this school are performing better than other students in the state, and this school is likely closing achievement gaps between different student groups.",
                  "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 291356,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Academic Progress",
                  "schoolGranularRatingValue": "6",
                  "schoolGranularRatingCopyText": "Students at this school are making average academic progress from one grade to the next compared to similar students in the state.",
                  "schoolGranularRatingsDescription": "The Academic Progress Rating is a growth proxy rating based on a model using unmatched cohorts, or school-level data instead of student-level data. The Academic Progress Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 291357,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "College Readiness",
                  "schoolGranularRatingValue": "10",
                  "schoolGranularRatingCopyText": "This school is above the state average in key measures of college and career readiness, such as graduation rates, college entrance exams, and advanced courses.\n\nEven at schools with strong college and career readiness, there may be students who are not getting the opportunities they need to succeed.\n",
                  "schoolGranularRatingsDescription": "The College Readiness Rating uses this high school's graduation rates, college entrance exam participation and performance, or AP, IB, or Dual Enrollment participation to determine how well schools are preparing students for success in college and beyond. The College Readiness Rating was created using the following data from the 2018 Civil Rights Data Collection: percentage of students enrolled in IB, AP or Dual Enrollment classes in grades 9-12, using 2019 SAT percent college ready data from California Department of Education, using 2019 Average ACT score data from California Department of Education, using 2019 ACT percent college ready data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 291355,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Test Scores",
                  "schoolGranularRatingValue": "8",
                  "schoolGranularRatingCopyText": "State test results at this school are above the state average, so students are likely performing at or above grade level.\n\nBecause state averages can be low, some students at this school may still not be performing at grade level.\n",
                  "schoolGranularRatingsDescription": "The Test Score Rating examines how students at this school performed on standardized tests compared with other schools in the state. The Test Rating was created using 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2022 California Science Test data from California Department of Education."
                }
              ],
              "schoolDistrict": {
                "id": 9940,
                "districtName": "Los Angeles Unified School District",
                "address": "333 South Beaudry Avenue",
                "city": "Los Angeles",
                "stateCode": "CA",
                "zip": "90017",
                "latitude": 34.056198,
                "longitude": -118.257172,
                "websiteUrl": "http://www.lausd.net",
                "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
              },
              "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/#Reviews",
              "elementary": true,
              "middle": true,
              "high": true,
              "lastUpdatedDate": "November 2023",
              "hasMultipleCatchmentAreas": false
            },
            {
              "servesHome": false,
              "greatSchoolsRating": 4,
              "parentRating": 5,
              "distanceInMiles": "12.4",
              "gradeRanges": "7-12",
              "institutionType": "Public",
              "name": "Thomas Riley High School",
              "schoolUrl": "/school/25245/CA/Los-Angeles/Thomas-Riley-High-School",
              "searchUrl": "/school/25245/CA/Los-Angeles/Thomas-Riley-High-School",
              "greatSchoolOverviewUrl": "https://www.greatschools.org/california/los-angeles/10951-Thomas-Riley-High-School/",
              "id": 25245,
              "numberOfStudents": 63,
              "fullAddress": "1524 E 103rd St, Los Angeles, CA 90002",
              "isChoice": true,
              "numReviews": 3,
              "studentToTeacherRatio": 8,
              "websiteUrl": "http://www.cde.ca.gov/re/sd/details.asp?cds=19647331930692&Public=Y",
              "schoolReviews": [
                {
                  "schoolId": 39595985,
                  "reviewedBy": "Other",
                  "datePosted": "January 2019",
                  "review": "I rate this school 5 stars because my experience at other schools was teachers didn't make their self available for 1 on 1 help like Thomas Riley High School",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610951,
                  "maponicsId": 3809515,
                  "url": "https://www.greatschools.org/california/los-angeles/10951-Thomas-Riley-High-School/"
                },
                {
                  "schoolId": 14560872,
                  "reviewedBy": "Parent",
                  "datePosted": "April 2011",
                  "review": "I went to Riley High School... I'm a student of class of 2008 graduate .. this is one of the best schools ever.... they will help you with everything you need... for the staff of thomas riley high school thank you for been there when i need it you guy's... GLORIA ESPARZA BAROCIO",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610951,
                  "maponicsId": 1089250,
                  "url": "https://www.greatschools.org/california/los-angeles/10951-Thomas-Riley-High-School/"
                },
                {
                  "schoolId": 8615798,
                  "reviewedBy": "Other",
                  "datePosted": "January 2011",
                  "review": "I am a former student and I would like to say that Riley High school was so caring and understanding in my time of need. I would also like to thank all of the staff for helping me during a very hard time in my life.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610951,
                  "maponicsId": 1052237,
                  "url": "https://www.greatschools.org/california/los-angeles/10951-Thomas-Riley-High-School/"
                }
              ],
              "schoolGranularRatings": [
                {
                  "schoolId": 322166,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Equity",
                  "schoolGranularRatingValue": "3",
                  "schoolGranularRatingCopyText": "Underserved students at this school may be falling behind other students in the state, and this school may have achievement gaps between different student groups.",
                  "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 322165,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "College Readiness",
                  "schoolGranularRatingValue": "4",
                  "schoolGranularRatingCopyText": "This school is below the state average in key measures of college and career readiness, such as graduation rates, college entrance exams, and advanced courses.",
                  "schoolGranularRatingsDescription": "The College Readiness Rating uses this high school's graduation rates, college entrance exam participation and performance, or AP, IB, or Dual Enrollment participation to determine how well schools are preparing students for success in college and beyond. The College Readiness Rating was created using the following data from the 2018 Civil Rights Data Collection: percentage of students enrolled in IB, AP or Dual Enrollment classes in grades 9-12, using 2019 SAT percent college ready data from California Department of Education, using 2019 Average ACT score data from California Department of Education, using 2019 ACT percent college ready data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                }
              ],
              "schoolDistrict": {
                "id": 9940,
                "districtName": "Los Angeles Unified School District",
                "address": "333 South Beaudry Avenue",
                "city": "Los Angeles",
                "stateCode": "CA",
                "zip": "90017",
                "latitude": 34.056198,
                "longitude": -118.257172,
                "websiteUrl": "http://www.lausd.net",
                "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
              },
              "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/los-angeles/10951-Thomas-Riley-High-School/#Reviews",
              "elementary": false,
              "middle": true,
              "high": true,
              "lastUpdatedDate": "November 2023",
              "hasMultipleCatchmentAreas": false
            },
            {
              "servesHome": false,
              "greatSchoolsRating": 9,
              "parentRating": 3,
              "distanceInMiles": "14.8",
              "gradeRanges": "K-12",
              "institutionType": "Charter",
              "name": "Granada Hills Charter High School",
              "schoolUrl": "/school/26677/CA/Los-Angeles/Granada-Hills-Charter-High-School",
              "searchUrl": "/school/26677/CA/Los-Angeles/Granada-Hills-Charter-High-School",
              "greatSchoolOverviewUrl": "https://www.greatschools.org/california/granada-hills/2112-Granada-Hills-Charter-High-School/",
              "id": 26677,
              "numberOfStudents": 4698,
              "fullAddress": "10535 Zelzah Ave, Granada Hills, CA 91344",
              "isChoice": true,
              "numReviews": 38,
              "studentToTeacherRatio": 27,
              "websiteUrl": "http://www.ghctk12.com",
              "schoolReviews": [
                {
                  "schoolId": 39556226,
                  "reviewedBy": "Student",
                  "datePosted": "September 2023",
                  "review": "this school sucks. First, they changed the nutrtion to 8 minutes. REALLY, 8 MINUTES? Nutrion is supposed to be for eating, going to the bathroom, and getting ready to go to your next class. I don't even have time to go to my usual spot where my friends and I hang out at, I BARELY have any time to even get a snack or even use the bathroom. Please note that this school has more than 6,000+ students and every single area of this school is EXTREMLY crowded. A lot of my teachers think this rule is crazy as well, and that students have no break. Do not get me wrong, some of the teachers here are amazing and teach in really good ways, the school has diverse cultures which I think is great as well. But i truly think that they dont give time for students to freshen up before their next class or to even catch a break at home. At first, i thought my brother was joking around when he said this school didn't care about us but he was right. I feel like this school is an okay option to go through, but there are a lot of fights and its super overcrowded. The bathroom rule is terrible too. i just want my nutriton back.",
                  "reviewerRating": 2,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602112,
                  "maponicsId": 6074543,
                  "url": "https://www.greatschools.org/california/granada-hills/2112-Granada-Hills-Charter-High-School/"
                },
                {
                  "schoolId": 39556225,
                  "reviewedBy": "Student",
                  "datePosted": "July 2023",
                  "review": "too many fights and too strict with bathroom rules and dress code",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602112,
                  "maponicsId": 6020688,
                  "url": "https://www.greatschools.org/california/granada-hills/2112-Granada-Hills-Charter-High-School/"
                },
                {
                  "schoolId": 39556224,
                  "reviewedBy": "Student",
                  "datePosted": "May 2023",
                  "review": "A school that feels more like a prison. Sets policies that are beyond strict for no reason. Was one of the happiest person out alive before I step foot into this high school. Absolutely atrocious and terrible.",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602112,
                  "maponicsId": 5941054,
                  "url": "https://www.greatschools.org/california/granada-hills/2112-Granada-Hills-Charter-High-School/"
                },
                {
                  "schoolId": 39556223,
                  "reviewedBy": "Student",
                  "datePosted": "May 2023",
                  "review": "A terrible school with terrible administration and staff. There are teachers that do not even care about your education.",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602112,
                  "maponicsId": 5928564,
                  "url": "https://www.greatschools.org/california/granada-hills/2112-Granada-Hills-Charter-High-School/"
                },
                {
                  "schoolId": 39556222,
                  "reviewedBy": "Parent",
                  "datePosted": "November 2022",
                  "review": "Good school with a variety of classes and programs. Sport teams are competitive. My only complaint is the absent late policy. We’re in the middle of a pandemic and you’re only allowed 15 days before you drop fail the class, I think this should be adjusted because it puts pressure on kids to show up for school, Rather than the motivation of wanting to be there. Complete difference. The dress code policy is also un-necessarily strict, stop objectifying kids for natural human things like shoulders and a mid riff.",
                  "reviewerRating": 3,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602112,
                  "maponicsId": 5658145,
                  "url": "https://www.greatschools.org/california/granada-hills/2112-Granada-Hills-Charter-High-School/"
                },
                {
                  "schoolId": 39556221,
                  "reviewedBy": "Other",
                  "datePosted": "June 2022",
                  "review": "Sad when Senior students are penalized for staying true to their own bodily autonomy and health standards.Blacklisting these young adults during the school year and excluding them from attending events that should be memories for the rest of their life, it is pathetic at best and reprehensible at worst. Discrimination such as this would never be allowed in any other capacity however, questionable \"science\" and flip flopping from so called \"experts\" throughout the last two years have brought into the spotlight just who the true lemmings are. Utterly ridiculous what the school board is being allowed to get away with but karma will come for them one day.If you have any other options available for educating your children, take it. Allowing this travesty to go on is only going to embolden them further in the future.",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602112,
                  "maponicsId": 5469505,
                  "url": "https://www.greatschools.org/california/granada-hills/2112-Granada-Hills-Charter-High-School/"
                },
                {
                  "schoolId": 39556220,
                  "reviewedBy": "Parent",
                  "datePosted": "May 2022",
                  "review": "People are fooled by the term independent charter, there is no such thing as we learned during COVID. This school like all others in the sewer that is LAUSD has Union controlled classrooms and district administrators oversight. COVID shots are less then 12% effective, yet the administration has NO PROBLEM bullying students who don't comply with political and medical tyranny. They are actually forbidding their seniors from walking and collecting their diplomas unless they submit. Send your kids to private school, homeschool, or pods… but keep them far away from this indoctrination center. And to the kids who did not comply. You, with your leadership, and independent thinking are genuinely our future. Congratulations.",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602112,
                  "maponicsId": 5454146,
                  "url": "https://www.greatschools.org/california/granada-hills/2112-Granada-Hills-Charter-High-School/"
                }
              ],
              "schoolGranularRatings": [
                {
                  "schoolId": 304155,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Equity",
                  "schoolGranularRatingValue": "9",
                  "schoolGranularRatingCopyText": "Underserved students at this school are performing better than other students in the state, and this school is likely closing achievement gaps between different student groups.",
                  "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 304159,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Test Scores",
                  "schoolGranularRatingValue": "9",
                  "schoolGranularRatingCopyText": "State test results at this school are above the state average, so students are likely performing at or above grade level.\n\nBecause state averages can be low, some students at this school may still not be performing at grade level.\n",
                  "schoolGranularRatingsDescription": "The Test Score Rating examines how students at this school performed on standardized tests compared with other schools in the state. The Test Rating was created using 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2022 California Science Test data from California Department of Education."
                },
                {
                  "schoolId": 304157,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "College Readiness",
                  "schoolGranularRatingValue": "10",
                  "schoolGranularRatingCopyText": "This school is above the state average in key measures of college and career readiness, such as graduation rates, college entrance exams, and advanced courses.\n\nEven at schools with strong college and career readiness, there may be students who are not getting the opportunities they need to succeed.\n",
                  "schoolGranularRatingsDescription": "The College Readiness Rating uses this high school's graduation rates, college entrance exam participation and performance, or AP, IB, or Dual Enrollment participation to determine how well schools are preparing students for success in college and beyond. The College Readiness Rating was created using the following data from the 2018 Civil Rights Data Collection: percentage of students enrolled in IB, AP or Dual Enrollment classes in grades 9-12, using 2019 SAT percent college ready data from California Department of Education, using 2019 Average ACT score data from California Department of Education, using 2019 ACT percent college ready data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                }
              ],
              "schoolDistrict": {
                "id": 9940,
                "districtName": "Los Angeles Unified School District",
                "address": "333 South Beaudry Avenue",
                "city": "Los Angeles",
                "stateCode": "CA",
                "zip": "90017",
                "latitude": 34.056198,
                "longitude": -118.257172,
                "websiteUrl": "http://www.lausd.net",
                "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
              },
              "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/granada-hills/2112-Granada-Hills-Charter-High-School/#Reviews",
              "elementary": true,
              "middle": true,
              "high": true,
              "lastUpdatedDate": "November 2023",
              "hasMultipleCatchmentAreas": false
            },
            {
              "servesHome": false,
              "greatSchoolsRating": 8,
              "parentRating": 4,
              "distanceInMiles": "16.0",
              "gradeRanges": "K-8",
              "institutionType": "Charter",
              "name": "Our Community Charter School",
              "schoolUrl": "/school/22803/CA/Los-Angeles/Our-Community-Charter-School",
              "searchUrl": "/school/22803/CA/Los-Angeles/Our-Community-Charter-School",
              "greatSchoolOverviewUrl": "https://www.greatschools.org/california/chatsworth/14935-Our-Community-Charter-School/",
              "id": 22803,
              "numberOfStudents": 447,
              "fullAddress": "10045 Jumilla Ave, Chatsworth, CA 91311",
              "isChoice": true,
              "numReviews": 27,
              "studentToTeacherRatio": 18,
              "websiteUrl": "http://ourcommunityschool.org",
              "schoolReviews": [
                {
                  "schoolId": 39608336,
                  "reviewedBy": "Parent",
                  "datePosted": "May 2023",
                  "review": "OCS is over-rated. It does feel safe and secluded, I’ll give them get 5 stars for that. The kids do make good friends and social development is important; but OCS focuses more on a sociological agenda than it does academics. OCS does not try to nurture students who have an aptitude or a desire for learning. Excellence is thwarted. Students don't get homework at this school. As my child got older, I realized the no-homework policy is so teachers don't have to grade school work at the expense of my child’s education. There is no testing for gifted students here. If your child might be gifted do not come here it will become impossible to move into a gifted school from here. OCS does all they can to treat all students equally; there is to be no bright shining stars at OCS, everyone has to be equal. This school is great for special ed students, the school does all they can to elevate special ed students.",
                  "reviewerRating": 3,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 614935,
                  "maponicsId": 5912404,
                  "url": "https://www.greatschools.org/california/chatsworth/14935-Our-Community-Charter-School/"
                },
                {
                  "schoolId": 39608335,
                  "reviewedBy": "Parent",
                  "datePosted": "November 2022",
                  "review": "Amazing school with amazing teachers that go above and beyond.They really care about their students and are not just there for a paycheck. We are very happy seeing how much our child is learning and loving going to school everyday.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 614935,
                  "maponicsId": 5671267,
                  "url": "https://www.greatschools.org/california/chatsworth/14935-Our-Community-Charter-School/"
                },
                {
                  "schoolId": 39608334,
                  "reviewedBy": "Other",
                  "datePosted": "March 2022",
                  "review": "Former employee-I have a disability and asked Vice Principal, Beth, for accommodations that would fallen under the umbrella of the ADA and was denied. She explained that I was clearly using my disability as a crutch (mind you I had never asked for anything and the accommodation I asked for would not have cost a cent) but she felt that I was using my disability as a way to excuse my incompetence. Disabilities are not one size fits all and Our Community needs to do a better job at accommodating employees with disabilities or perhaps read up on the ADA before denying a disabled employee their rights.",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 614935,
                  "maponicsId": 5355784,
                  "url": "https://www.greatschools.org/california/chatsworth/14935-Our-Community-Charter-School/"
                },
                {
                  "schoolId": 39608333,
                  "reviewedBy": "Parent",
                  "datePosted": "November 2021",
                  "review": "This school advertised itself as a school that understands disability and diversity; however, our experience proved otherwise. The teachers did not show any understanding of disability and blamed my child for her difficulties. The BID (the aid’s supervisor) did not have appropriate training to supervise the BII. The BID (supervisor) engaged in a multiple relationship with my child, being her counselor and her BID at the same time, which is unethical. The children were bullying my child and the teachers and administration blamed my child for it. The assistant principal was not responsive to our needs. We had to pull our child out of the school in the middle of the semester because she was suffering there and was not learning anything.",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 614935,
                  "maponicsId": 5178076,
                  "url": "https://www.greatschools.org/california/chatsworth/14935-Our-Community-Charter-School/"
                },
                {
                  "schoolId": 39608332,
                  "reviewedBy": "Parent",
                  "datePosted": "May 2021",
                  "review": "Five years in, we continue to feel so grateful for this school even with some ups and downs over the years. As parents, we appreciate the community so much-- including the staff as well as the other families that we've met there. But much more important, both of our kids love going to school every day, which was not true *at all* when our son started kindergarten at our neighborhood school. We switched as soon as we had the chance, and witnessed first hand the difference that the OCS commitment to social and emotional wellness makes. And we are so grateful for the hard work, the dedication, and the difficult decision-making the administration put into reopening after closing due to COVID. Our kids are thrilled to be back at OCS, and so are we!",
                  "reviewerRating": 4,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 614935,
                  "maponicsId": 4968348,
                  "url": "https://www.greatschools.org/california/chatsworth/14935-Our-Community-Charter-School/"
                },
                {
                  "schoolId": 39608331,
                  "reviewedBy": "Parent",
                  "datePosted": "June 2019",
                  "review": "It used to be a wonderful school, a community of parents and children. But the school leadership has been destroying that community since the current executive director was hired as principal. The education was good, but if you're expecting a progressive education you'll be disappointed. It's a traditional school trying to promote itself as if it's something else.",
                  "reviewerRating": 2,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 614935,
                  "maponicsId": 4082635,
                  "url": "https://www.greatschools.org/california/chatsworth/14935-Our-Community-Charter-School/"
                },
                {
                  "schoolId": 39608330,
                  "reviewedBy": "Parent",
                  "datePosted": "June 2019",
                  "review": "It was beautiful, my children loved it, but sadly is not the same",
                  "reviewerRating": 2,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 614935,
                  "maponicsId": 4070389,
                  "url": "https://www.greatschools.org/california/chatsworth/14935-Our-Community-Charter-School/"
                }
              ],
              "schoolGranularRatings": [
                {
                  "schoolId": 286801,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Test Scores",
                  "schoolGranularRatingValue": "9",
                  "schoolGranularRatingCopyText": "State test results at this school are above the state average, so students are likely performing at or above grade level.\n\nBecause state averages can be low, some students at this school may still not be performing at grade level.\n",
                  "schoolGranularRatingsDescription": "The Test Score Rating examines how students at this school performed on standardized tests compared with other schools in the state. The Test Rating was created using 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2022 California Science Test data from California Department of Education."
                },
                {
                  "schoolId": 286799,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Equity",
                  "schoolGranularRatingValue": "9",
                  "schoolGranularRatingCopyText": "Underserved students at this school are performing better than other students in the state, and this school is likely closing achievement gaps between different student groups.",
                  "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 286800,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Academic Progress",
                  "schoolGranularRatingValue": "6",
                  "schoolGranularRatingCopyText": "Students at this school are making average academic progress from one grade to the next compared to similar students in the state.",
                  "schoolGranularRatingsDescription": "The Academic Progress Rating is a growth proxy rating based on a model using unmatched cohorts, or school-level data instead of student-level data. The Academic Progress Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2019 demographic data from California Department of Education."
                }
              ],
              "schoolDistrict": {
                "id": 9940,
                "districtName": "Los Angeles Unified School District",
                "address": "333 South Beaudry Avenue",
                "city": "Los Angeles",
                "stateCode": "CA",
                "zip": "90017",
                "latitude": 34.056198,
                "longitude": -118.257172,
                "websiteUrl": "http://www.lausd.net",
                "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
              },
              "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/chatsworth/14935-Our-Community-Charter-School/#Reviews",
              "elementary": true,
              "middle": true,
              "high": false,
              "lastUpdatedDate": "November 2023",
              "hasMultipleCatchmentAreas": false
            }
          ],
          "highChoiceSchoolsInDistrict": [
            {
              "servesHome": false,
              "greatSchoolsRating": 3,
              "parentRating": 4,
              "distanceInMiles": "1.4",
              "gradeRanges": "9-12",
              "institutionType": "Public",
              "name": "Whitman Continuation School",
              "schoolUrl": "/school/23155/CA/Los-Angeles/Whitman-Continuation-School",
              "searchUrl": "/school/23155/CA/Los-Angeles/Whitman-Continuation-School",
              "greatSchoolOverviewUrl": "https://www.greatschools.org/california/los-angeles/2489-Whitman-Continuation-School/",
              "id": 23155,
              "numberOfStudents": 70,
              "fullAddress": "7795 Rosewood Ave, Los Angeles, CA 90036",
              "isChoice": true,
              "numReviews": 2,
              "studentToTeacherRatio": 22,
              "websiteUrl": "http://www.whitmanhs.org",
              "schoolReviews": [
                {
                  "schoolId": 39558184,
                  "reviewedBy": "Other",
                  "datePosted": "February 2017",
                  "review": "Rigorous curriculumDedicated and Caring Teachers and AdministrationSafe and positive culture conducive to learning",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602489,
                  "maponicsId": 2478301,
                  "url": "https://www.greatschools.org/california/los-angeles/2489-Whitman-Continuation-School/"
                },
                {
                  "schoolId": 8520677,
                  "reviewedBy": "Student",
                  "datePosted": "February 2010",
                  "review": "N/A",
                  "reviewerRating": 3,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602489,
                  "maponicsId": 886416,
                  "url": "https://www.greatschools.org/california/los-angeles/2489-Whitman-Continuation-School/"
                }
              ],
              "schoolGranularRatings": [
                {
                  "schoolId": 360079,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "College Readiness",
                  "schoolGranularRatingValue": "4",
                  "schoolGranularRatingCopyText": "This school is below the state average in key measures of college and career readiness, such as graduation rates, college entrance exams, and advanced courses.",
                  "schoolGranularRatingsDescription": "The College Readiness Rating uses this high school's graduation rates, college entrance exam participation and performance, or AP, IB, or Dual Enrollment participation to determine how well schools are preparing students for success in college and beyond. The College Readiness Rating was created using the following data from the 2018 Civil Rights Data Collection: percentage of students enrolled in IB, AP or Dual Enrollment classes in grades 9-12, using 2019 SAT percent college ready data from California Department of Education, using 2019 Average ACT score data from California Department of Education, using 2019 ACT percent college ready data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 360078,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Equity",
                  "schoolGranularRatingValue": "2",
                  "schoolGranularRatingCopyText": "Underserved students at this school may be falling behind other students in the state, and this school may have achievement gaps between different student groups.",
                  "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                }
              ],
              "schoolDistrict": {
                "id": 9940,
                "districtName": "Los Angeles Unified School District",
                "address": "333 South Beaudry Avenue",
                "city": "Los Angeles",
                "stateCode": "CA",
                "zip": "90017",
                "latitude": 34.056198,
                "longitude": -118.257172,
                "websiteUrl": "http://www.lausd.net",
                "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
              },
              "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/los-angeles/2489-Whitman-Continuation-School/#Reviews",
              "elementary": false,
              "middle": false,
              "high": true,
              "lastUpdatedDate": "November 2023",
              "hasMultipleCatchmentAreas": false
            },
            {
              "servesHome": false,
              "greatSchoolsRating": 3,
              "parentRating": 3,
              "distanceInMiles": "10.5",
              "gradeRanges": "9-12",
              "institutionType": "Public",
              "name": "Independence Continuation School",
              "schoolUrl": "/school/23310/CA/Los-Angeles/Independence-Continuation-School",
              "searchUrl": "/school/23310/CA/Los-Angeles/Independence-Continuation-School",
              "greatSchoolOverviewUrl": "https://www.greatschools.org/california/lake-balboa/2160-Independence-Continuation-School/",
              "id": 23310,
              "numberOfStudents": 112,
              "fullAddress": "6501 Balboa Blvd, Van Nuys, CA 91406",
              "isChoice": true,
              "numReviews": 2,
              "studentToTeacherRatio": 14,
              "websiteUrl": "http://ichs-lausd-ca.schoolloop.com",
              "schoolReviews": [
                {
                  "schoolId": 8515914,
                  "reviewedBy": "Other",
                  "datePosted": "August 2011",
                  "review": "this school was amazing when i attended, only reason i graduated! the staff is very supportive of the students.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602160,
                  "maponicsId": 1124873,
                  "url": "https://www.greatschools.org/california/lake-balboa/2160-Independence-Continuation-School/"
                },
                {
                  "schoolId": 8515913,
                  "reviewedBy": "Parent",
                  "datePosted": "May 2008",
                  "review": "N/A",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602160,
                  "maponicsId": 563653,
                  "url": "https://www.greatschools.org/california/lake-balboa/2160-Independence-Continuation-School/"
                }
              ],
              "schoolGranularRatings": [
                {
                  "schoolId": 360094,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Equity",
                  "schoolGranularRatingValue": "2",
                  "schoolGranularRatingCopyText": "Underserved students at this school may be falling behind other students in the state, and this school may have achievement gaps between different student groups.",
                  "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 360095,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "College Readiness",
                  "schoolGranularRatingValue": "3",
                  "schoolGranularRatingCopyText": "This school is below the state average in key measures of college and career readiness, such as graduation rates, college entrance exams, and advanced courses.",
                  "schoolGranularRatingsDescription": "The College Readiness Rating uses this high school's graduation rates, college entrance exam participation and performance, or AP, IB, or Dual Enrollment participation to determine how well schools are preparing students for success in college and beyond. The College Readiness Rating was created using the following data from the 2018 Civil Rights Data Collection: percentage of students enrolled in IB, AP or Dual Enrollment classes in grades 9-12, using 2019 SAT percent college ready data from California Department of Education, using 2019 Average ACT score data from California Department of Education, using 2019 ACT percent college ready data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                }
              ],
              "schoolDistrict": {
                "id": 9940,
                "districtName": "Los Angeles Unified School District",
                "address": "333 South Beaudry Avenue",
                "city": "Los Angeles",
                "stateCode": "CA",
                "zip": "90017",
                "latitude": 34.056198,
                "longitude": -118.257172,
                "websiteUrl": "http://www.lausd.net",
                "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
              },
              "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/lake-balboa/2160-Independence-Continuation-School/#Reviews",
              "elementary": false,
              "middle": false,
              "high": true,
              "lastUpdatedDate": "November 2023",
              "hasMultipleCatchmentAreas": false
            },
            {
              "servesHome": false,
              "greatSchoolsRating": 8,
              "parentRating": 3,
              "distanceInMiles": "10.6",
              "gradeRanges": "K-12",
              "institutionType": "Public",
              "name": "Lake Balboa College Preparatory Magnet K-12",
              "schoolUrl": "/school/22921/CA/Los-Angeles/Lake-Balboa-College-Preparatory-Magnet-K-12",
              "searchUrl": "/school/22921/CA/Los-Angeles/Lake-Balboa-College-Preparatory-Magnet-K-12",
              "greatSchoolOverviewUrl": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/",
              "id": 22921,
              "numberOfStudents": 582,
              "fullAddress": "6701 Balboa Blvd, Lake Balboa, CA 91406",
              "isChoice": true,
              "numReviews": 29,
              "studentToTeacherRatio": 22,
              "websiteUrl": "https://www.lakebalboacollegeprep.com/",
              "schoolReviews": [
                {
                  "schoolId": 39595953,
                  "reviewedBy": "Parent",
                  "datePosted": "May 2023",
                  "review": "My child has attended this school for quite some time and it just keeps getting worst especially ever since the current principal Mr. Clark took over. If you ever have any safety concerns or want to discuss anything about your child or the teachers with him he cuts you off never has the time to talk to you or he just gives you the run around. Also the teachers just let the students do whatever they miss assignments and they can just do it when ever they want. I agree with previous reviews this school definitely has a problem with bullying and Mr. Clark and staff will not do anything about it. Its nice that is a small school compared to others but you are better off taking your child elsewhere.",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610954,
                  "maponicsId": 5895254,
                  "url": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/"
                },
                {
                  "schoolId": 39595952,
                  "reviewedBy": "Parent",
                  "datePosted": "March 2023",
                  "review": "I am a Student at this school and there are people sneaking out and the teachers don't really care. If students do something wrong, there are no staff at the playground and the second graders already know to many slurs and the highschool kids are teaching them.",
                  "reviewerRating": 2,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610954,
                  "maponicsId": 5814854,
                  "url": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/"
                },
                {
                  "schoolId": 39595951,
                  "reviewedBy": "Parent",
                  "datePosted": "July 2022",
                  "review": "Not a good school at all always assuming things and end up looking dumb. Principle does not do anything about bullying.",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610954,
                  "maponicsId": 5523769,
                  "url": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/"
                },
                {
                  "schoolId": 39595950,
                  "reviewedBy": "Teacher",
                  "datePosted": "January 2022",
                  "review": "I believe this school is amazing and has definitely expanded in the past few years. There are now 6 different AP (college prep) classes: Chemistry, English Literature, English Language, Psychology, U.S. History, and World History. There is also a lovely farm that allows students to learn responsibility and how to care for animals. It is a very unique Kindergarten through 12th grade school that is relatively small, allowing for a feeling of family and individual attention to all students.",
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610954,
                  "maponicsId": 5263100,
                  "url": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/"
                },
                {
                  "schoolId": 39595949,
                  "reviewedBy": "Parent",
                  "datePosted": "October 2018",
                  "review": "I love this school i bonded with the teachers. Yes some teachers are not that good but most of them are effective and care about you.",
                  "reviewerRating": 4,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610954,
                  "maponicsId": 3653886,
                  "url": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/"
                },
                {
                  "schoolId": 39595948,
                  "reviewedBy": "Other",
                  "datePosted": "June 2017",
                  "review": "I have written about this school years before. Now that I have graduated, I can surely say this school was the worst high school experience ever. A majority of the staff do not care about the students and are quite rude to them. A particular coordinator would not even say \"hi\" back when I would say hi to them. Too much drama, too much of adults acting like children, and most teachers don't even teach except for the science teachers and the government teacher, they are great. Please don't take your kids here. I and others really did not have a good experience with these people.",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610954,
                  "maponicsId": 2717034,
                  "url": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/"
                },
                {
                  "schoolId": 39595947,
                  "reviewedBy": "Student",
                  "datePosted": "January 2017",
                  "review": "I had a great experience at this school.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610954,
                  "maponicsId": 2433179,
                  "url": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/"
                }
              ],
              "schoolGranularRatings": [
                {
                  "schoolId": 287261,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Equity",
                  "schoolGranularRatingValue": "7",
                  "schoolGranularRatingCopyText": "Underserved students at this school are performing better than other students in the state, and this school is likely closing achievement gaps between different student groups.",
                  "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 287262,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "College Readiness",
                  "schoolGranularRatingValue": "9",
                  "schoolGranularRatingCopyText": "This school is above the state average in key measures of college and career readiness, such as graduation rates, college entrance exams, and advanced courses.\n\nEven at schools with strong college and career readiness, there may be students who are not getting the opportunities they need to succeed.\n",
                  "schoolGranularRatingsDescription": "The College Readiness Rating uses this high school's graduation rates, college entrance exam participation and performance, or AP, IB, or Dual Enrollment participation to determine how well schools are preparing students for success in college and beyond. The College Readiness Rating was created using the following data from the 2018 Civil Rights Data Collection: percentage of students enrolled in IB, AP or Dual Enrollment classes in grades 9-12, using 2019 SAT percent college ready data from California Department of Education, using 2019 Average ACT score data from California Department of Education, using 2019 ACT percent college ready data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 287263,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Academic Progress",
                  "schoolGranularRatingValue": "7",
                  "schoolGranularRatingCopyText": "Students at this school are making more academic progress from one grade to the next compared to similar students in the state.",
                  "schoolGranularRatingsDescription": "The Academic Progress Rating is a growth proxy rating based on a model using unmatched cohorts, or school-level data instead of student-level data. The Academic Progress Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2019 demographic data from California Department of Education."
                }
              ],
              "schoolDistrict": {
                "id": 9940,
                "districtName": "Los Angeles Unified School District",
                "address": "333 South Beaudry Avenue",
                "city": "Los Angeles",
                "stateCode": "CA",
                "zip": "90017",
                "latitude": 34.056198,
                "longitude": -118.257172,
                "websiteUrl": "http://www.lausd.net",
                "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
              },
              "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/lake-balboa/10954-Valley-Alternative-Magnet/#Reviews",
              "elementary": true,
              "middle": true,
              "high": true,
              "lastUpdatedDate": "November 2023",
              "hasMultipleCatchmentAreas": false
            },
            {
              "servesHome": false,
              "greatSchoolsRating": 5,
              "parentRating": 5,
              "distanceInMiles": "11.0",
              "gradeRanges": "9-12",
              "institutionType": "Public",
              "name": "San Antonio Continuation School",
              "schoolUrl": "/school/25210/CA/Huntington-Park/San-Antonio-Continuation-School",
              "searchUrl": "/school/25210/CA/Huntington-Park/San-Antonio-Continuation-School",
              "greatSchoolOverviewUrl": "https://www.greatschools.org/california/huntington-park/2363-San-Antonio-Continuation-School/",
              "id": 25210,
              "numberOfStudents": 161,
              "fullAddress": "2911 Belgrave Ave, Huntington Park, CA 90255",
              "isChoice": true,
              "numReviews": 4,
              "studentToTeacherRatio": 21,
              "websiteUrl": "http://sanantoniohs-lausd-ca.schoolloop.com",
              "schoolReviews": [
                {
                  "schoolId": 39557571,
                  "reviewedBy": "Parent",
                  "datePosted": "November 2022",
                  "review": "Small comfortable school that let you work at your own pace.",
                  "reviewerRating": 4,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602363,
                  "maponicsId": 5667797,
                  "url": "https://www.greatschools.org/california/huntington-park/2363-San-Antonio-Continuation-School/"
                },
                {
                  "schoolId": 14512067,
                  "reviewedBy": "Other",
                  "datePosted": "September 2014",
                  "review": "San Antonio High school was the best choice i ever made after making bad decisions in my former high school. Thanks to the helpful staff and teachers i was able to become responsible and independent for myself and studies, which I'm eternally thankful for since it made my college life more easy. The teachers also provide great tutoring for any subject and have a great sense of humor with the students. I'm a proud 2013 graduate from this great high school and a successful college student. I just highly advise to please over look the SAT's and have your son/daughter speak with staff and helpful teachers about it in S.A.H.S. as soon as possible. If no interest is shown, the S.A.T's won't be provided for him/her if it's to late to register for the test. I know this from my past experience.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602363,
                  "maponicsId": 1528551,
                  "url": "https://www.greatschools.org/california/huntington-park/2363-San-Antonio-Continuation-School/"
                },
                {
                  "schoolId": 8518688,
                  "reviewedBy": "Parent",
                  "datePosted": "August 2011",
                  "review": "Concerned for my son education, I looked for the best available HS in the area. I found out that SAHS had made the API for 4 years and the AYP for three years in a row. SAHS is also accredited by the Western Association of School and Colleges. The school recieved an unheard 6-year accreditation by WASC. This speaks volumes about the school. Their academic program is student centered and the atmosphere is simply the best you can find in any school. I highly recommend this school for your children. By the way, SAHS is the only school that has made the API & AYP in LAUSD Local District 6.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602363,
                  "maponicsId": 1128891,
                  "url": "https://www.greatschools.org/california/huntington-park/2363-San-Antonio-Continuation-School/"
                },
                {
                  "schoolId": 14512066,
                  "reviewedBy": "Parent",
                  "datePosted": "March 2010",
                  "review": "As a concerned parent, I can honestly say that SAHS is one of the best schools in LAUSD. The caring atmosphere is unique; this is a student center school whose results are superior to other schools in the area.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 602363,
                  "maponicsId": 931010,
                  "url": "https://www.greatschools.org/california/huntington-park/2363-San-Antonio-Continuation-School/"
                }
              ],
              "schoolGranularRatings": [
                {
                  "schoolId": 360536,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Equity",
                  "schoolGranularRatingValue": "3",
                  "schoolGranularRatingCopyText": "Underserved students at this school may be falling behind other students in the state, and this school may have achievement gaps between different student groups.",
                  "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 360535,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "College Readiness",
                  "schoolGranularRatingValue": "2",
                  "schoolGranularRatingCopyText": "This school is below the state average in key measures of college and career readiness, such as graduation rates, college entrance exams, and advanced courses.",
                  "schoolGranularRatingsDescription": "The College Readiness Rating uses this high school's graduation rates, college entrance exam participation and performance, or AP, IB, or Dual Enrollment participation to determine how well schools are preparing students for success in college and beyond. The College Readiness Rating was created using the following data from the 2018 Civil Rights Data Collection: percentage of students enrolled in IB, AP or Dual Enrollment classes in grades 9-12, using 2019 SAT percent college ready data from California Department of Education, using 2019 Average ACT score data from California Department of Education, using 2019 ACT percent college ready data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 360537,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Academic Progress",
                  "schoolGranularRatingValue": "10",
                  "schoolGranularRatingCopyText": "Students at this school are making more academic progress from one grade to the next compared to similar students in the state.",
                  "schoolGranularRatingsDescription": "The Academic Progress Rating is a growth proxy rating based on a model using unmatched cohorts, or school-level data instead of student-level data. The Academic Progress Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2019 demographic data from California Department of Education."
                }
              ],
              "schoolDistrict": {
                "id": 9940,
                "districtName": "Los Angeles Unified School District",
                "address": "333 South Beaudry Avenue",
                "city": "Los Angeles",
                "stateCode": "CA",
                "zip": "90017",
                "latitude": 34.056198,
                "longitude": -118.257172,
                "websiteUrl": "http://www.lausd.net",
                "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
              },
              "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/huntington-park/2363-San-Antonio-Continuation-School/#Reviews",
              "elementary": false,
              "middle": false,
              "high": true,
              "lastUpdatedDate": "November 2023",
              "hasMultipleCatchmentAreas": false
            },
            {
              "servesHome": false,
              "greatSchoolsRating": 8,
              "parentRating": 3,
              "distanceInMiles": "12.1",
              "gradeRanges": "4-12",
              "institutionType": "Public",
              "name": "Sherman Oaks Center For Enriched Studies",
              "schoolUrl": "/school/24145/CA/Los-Angeles/Sherman-Oaks-Center-For-Enriched-Studies",
              "searchUrl": "/school/24145/CA/Los-Angeles/Sherman-Oaks-Center-For-Enriched-Studies",
              "greatSchoolOverviewUrl": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/",
              "id": 24145,
              "numberOfStudents": 2087,
              "fullAddress": "18605 Erwin St, Reseda, CA 91335",
              "isChoice": true,
              "numReviews": 28,
              "studentToTeacherRatio": 30,
              "websiteUrl": "http://www.shermanoaksces.com",
              "schoolReviews": [
                {
                  "schoolId": 39595964,
                  "reviewedBy": "Parent",
                  "datePosted": "May 2023",
                  "review": "this school is literally insane and im not being overdramatic some of the teachers are literally pervs and need to get fired but since they cant find anyone they cant fire them all the students are smelly and I'm not joking they stink and they're so mean and gross people overall this school needs like a wellness check or smth there's only like two good teachers here and they deserve better and so do we thanks don't send ur children here",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610957,
                  "maponicsId": 5927431,
                  "url": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/"
                },
                {
                  "schoolId": 39595963,
                  "reviewedBy": "Student",
                  "datePosted": "January 2021",
                  "review": "I have been attending this school for over six years now and I feel as though it is necessary to write a review. This school is academically standard, but it is promising that most of the students here will get to a university or college of their choice.Homework: Some teachers take it upon themselves to assign endless amounts of homework, giving students barely enough time to get the amount of rest is needed. There are 3-6 classes a day and we are given so much homework from each of them, it’s no wonder why we’re exhausted. Teachers (and staff)In addition, the majority staff and teachers rarely provide any care to problems such as bullying or a problematic teacher/sub assigning the most ridiculous work. Even negative online activity isn’t even being looked upon simply because the staff “isn’t being paid enough” to do anything about it. Now, with online learning, the teachers are assigning work that they don’t even teach. Why is the synchronous class time so long then?LeadershipI don’t have much to say about leadership but there are activities where students and parents can be involved in. The new principal shows promising leadership but we’ll see what happens. CharacterRegarding character, SOCES has a lot of rules but they’re not really enforced so we are mostly free with what we do, kind of like what happens if you were out in town. It develops who we are as people in the world outside of school. Learning DifferencesAlthough we go over learning differences in many classes, there just isn’t enough that teachers do to support all of them. They just want to get over with the classes but occasionally they help. BullyingThe staff “isn’t being paid enough” to give much regard to bullying, especially cyberbullying. In cases that there is bullying, both parties get sent to the counselor and get a measly warning.",
                  "reviewerRating": 3,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610957,
                  "maponicsId": 4807442,
                  "url": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/"
                },
                {
                  "schoolId": 39595962,
                  "reviewedBy": "Parent",
                  "datePosted": "November 2019",
                  "review": "Overall Good - ...The administration need to have better customer services with our kids and parents. The teachers need to not give out excessive homework, classrooms should be formatted smaller on the student to teacher ratio and the school needs a make-over, which is occurring now.",
                  "reviewerRating": 3,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610957,
                  "maponicsId": 4277208,
                  "url": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/"
                },
                {
                  "schoolId": 39595961,
                  "reviewedBy": "Parent",
                  "datePosted": "April 2019",
                  "review": "The school didn't meet our expectations. It seems like the perfect school with high ratings, but there's lots of bullying and the administration buries all their issues. There is always some kind of problem that no one ever fixes. There are some amazing teachers who care about the student. I can't say the same about the other staff.",
                  "reviewerRating": 2,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610957,
                  "maponicsId": 3956937,
                  "url": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/"
                },
                {
                  "schoolId": 39595960,
                  "reviewedBy": "Student",
                  "datePosted": "March 2019",
                  "review": "This school is wonderful in learning and character and I give it 5 stars!!! I am in 5 th grade and my math and science/ health teacher is one of the most amazing, kind, and inspiring educators ever!! Her name is Mrs. Gibson and all the kids in my class love her! We do a lot of fun and educational projects.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610957,
                  "maponicsId": 3924523,
                  "url": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/"
                },
                {
                  "schoolId": 39595959,
                  "reviewedBy": "Parent",
                  "datePosted": "April 2018",
                  "review": "My daughter loves it here. I love it overall. They have great programs and electives. The school's booster club and PTA needs to back off the staff and parents for money though. I get it, the kids need money for field trips. But my kid has been in the school for 3 years and has taken only one trip, so where is the money really going? Aside from that. LOVE THIS SCHOOL!",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610957,
                  "maponicsId": 3330516,
                  "url": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/"
                },
                {
                  "schoolId": 39595958,
                  "reviewedBy": "Parent",
                  "datePosted": "June 2017",
                  "review": "In our experience, it was ok. Its a safe place but its not the 'private' school of the public schools like it once was. The well being of the students does not come first at this school. Its all about what is convenient for the teachers.",
                  "reviewerRating": 1,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 610957,
                  "maponicsId": 2683450,
                  "url": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/"
                }
              ],
              "schoolGranularRatings": [
                {
                  "schoolId": 291359,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Equity",
                  "schoolGranularRatingValue": "7",
                  "schoolGranularRatingCopyText": "Underserved students at this school are performing better than other students in the state, and this school is likely closing achievement gaps between different student groups.",
                  "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 291356,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Academic Progress",
                  "schoolGranularRatingValue": "6",
                  "schoolGranularRatingCopyText": "Students at this school are making average academic progress from one grade to the next compared to similar students in the state.",
                  "schoolGranularRatingsDescription": "The Academic Progress Rating is a growth proxy rating based on a model using unmatched cohorts, or school-level data instead of student-level data. The Academic Progress Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 291357,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "College Readiness",
                  "schoolGranularRatingValue": "10",
                  "schoolGranularRatingCopyText": "This school is above the state average in key measures of college and career readiness, such as graduation rates, college entrance exams, and advanced courses.\n\nEven at schools with strong college and career readiness, there may be students who are not getting the opportunities they need to succeed.\n",
                  "schoolGranularRatingsDescription": "The College Readiness Rating uses this high school's graduation rates, college entrance exam participation and performance, or AP, IB, or Dual Enrollment participation to determine how well schools are preparing students for success in college and beyond. The College Readiness Rating was created using the following data from the 2018 Civil Rights Data Collection: percentage of students enrolled in IB, AP or Dual Enrollment classes in grades 9-12, using 2019 SAT percent college ready data from California Department of Education, using 2019 Average ACT score data from California Department of Education, using 2019 ACT percent college ready data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 291355,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Test Scores",
                  "schoolGranularRatingValue": "8",
                  "schoolGranularRatingCopyText": "State test results at this school are above the state average, so students are likely performing at or above grade level.\n\nBecause state averages can be low, some students at this school may still not be performing at grade level.\n",
                  "schoolGranularRatingsDescription": "The Test Score Rating examines how students at this school performed on standardized tests compared with other schools in the state. The Test Rating was created using 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2022 California Science Test data from California Department of Education."
                }
              ],
              "schoolDistrict": {
                "id": 9940,
                "districtName": "Los Angeles Unified School District",
                "address": "333 South Beaudry Avenue",
                "city": "Los Angeles",
                "stateCode": "CA",
                "zip": "90017",
                "latitude": 34.056198,
                "longitude": -118.257172,
                "websiteUrl": "http://www.lausd.net",
                "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
              },
              "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/reseda/10957-Sherman-Oaks-Center-For-Enriched-Studies/#Reviews",
              "elementary": true,
              "middle": true,
              "high": true,
              "lastUpdatedDate": "November 2023",
              "hasMultipleCatchmentAreas": false
            },
            {
              "servesHome": false,
              "greatSchoolsRating": 8,
              "parentRating": 4,
              "distanceInMiles": "25.1",
              "gradeRanges": "9-12",
              "institutionType": "Charter",
              "name": "Port of Los Angeles High School",
              "schoolUrl": "/school/22827/CA/Los-Angeles/Port-of-Los-Angeles-High-School",
              "searchUrl": "/school/22827/CA/Los-Angeles/Port-of-Los-Angeles-High-School",
              "greatSchoolOverviewUrl": "https://www.greatschools.org/california/san-pedro/14264-Port-Of-Los-Angeles-High-School/",
              "id": 22827,
              "numberOfStudents": 968,
              "fullAddress": "250 W 5th St, San Pedro, CA 90731",
              "isChoice": true,
              "numReviews": 14,
              "studentToTeacherRatio": 16,
              "websiteUrl": "http://polahs.net",
              "schoolReviews": [
                {
                  "schoolId": 39606448,
                  "reviewedBy": "Parent",
                  "datePosted": "March 2023",
                  "review": "This school has changed dramatically in the last 3 years. We no longer recommend the school and feel we dodged a bullet because our daughter graduated 2 years ago, but our nephew still attends. The school is lacking in a reality check or something. Too much nonsense with academics and no social outlets worth partaking in according to our nephew. He looks bored and sad. I hope the best for the school but the leaders are missing so many points that need change, or the school will fail the kids.",
                  "reviewerRating": 3,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 614264,
                  "maponicsId": 5827288,
                  "url": "https://www.greatschools.org/california/san-pedro/14264-Port-Of-Los-Angeles-High-School/"
                },
                {
                  "schoolId": 39606447,
                  "reviewedBy": "Parent",
                  "datePosted": "April 2022",
                  "review": "Teachers at this school are very helpful, but are VERY strict. I can see some potential but the strictness ruins the school spirit. I have gone far and became better with my education in this school but i can not name something fun that came out of this school. Teachers also give a crazy amount of homework; assigning homework while already having homework. Really hard catching up when giving a ridiculous amount of work.",
                  "reviewerRating": 2,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 614264,
                  "maponicsId": 5384681,
                  "url": "https://www.greatschools.org/california/san-pedro/14264-Port-Of-Los-Angeles-High-School/"
                },
                {
                  "schoolId": 39606446,
                  "reviewedBy": "Other",
                  "datePosted": "October 2021",
                  "review": "If you are looking for a high school experience DON’T go here. There are only a few teachers that are great at teaching. This school has no spirit AT ALL. I believe that a high school should set rules and allow students to have fun but this school is beyond strict and as the worst punishment for the smallest things like not wearing your polo under a hoodie, and learning here isn’t fun any more. This school gives an exaggerating amount of homework.",
                  "reviewerRating": 2,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 614264,
                  "maponicsId": 5150876,
                  "url": "https://www.greatschools.org/california/san-pedro/14264-Port-Of-Los-Angeles-High-School/"
                },
                {
                  "schoolId": 39606445,
                  "reviewedBy": "Parent",
                  "datePosted": "August 2018",
                  "review": "My son has never been happier and is truly thriving in high school. We suffered thru middle school, and he completely changed everything around at POLAH. The small campus definitely benefits my son and he has so much support from teachers after school for tutoring. I couldn't be happier, and the teachers truly show how involved they are in making sure each student succeeds in life. Their approach to teaching has definitely made an impact and we are so excited with all my son has learned.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 614264,
                  "maponicsId": 3593867,
                  "url": "https://www.greatschools.org/california/san-pedro/14264-Port-Of-Los-Angeles-High-School/"
                },
                {
                  "schoolId": 39606444,
                  "reviewedBy": "Parent",
                  "datePosted": "November 2015",
                  "review": "All the staff at POLAHS put the student's success first and go out of their way to encourage all students to participate in their education.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 614264,
                  "maponicsId": 1850631,
                  "url": "https://www.greatschools.org/california/san-pedro/14264-Port-Of-Los-Angeles-High-School/"
                },
                {
                  "schoolId": 39606443,
                  "reviewedBy": "Parent",
                  "datePosted": "October 2015",
                  "review": "This school is amazing. My son is in the 9th grade at POLAHS and I am super impressed with the education, faculty & facilities. Mr Scotti and Mr Cosgrove are attentive and caring, they make sure our kids are safe by being present outside everyday. The education is equivalent to any private education you would receive. J. Correa RN",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 614264,
                  "maponicsId": 1786517,
                  "url": "https://www.greatschools.org/california/san-pedro/14264-Port-Of-Los-Angeles-High-School/"
                },
                {
                  "schoolId": 5542039,
                  "reviewedBy": "Other",
                  "datePosted": "April 2015",
                  "review": "I am a proud alumni of Port of Los Angeles High School and Varsity Cheer. After reading all of the reviews on this site, I would like to present several rebuttals. There may be a lot of homework given at the school, but all of the students have a sufficient amount of time to complete it. The school runs on what is called a \"block-schedule.\" This means that students attend all 6 classes on Monday, 1-3 periods on Tuesday and Thursday, and 4-6 periods on Wednesday and Friday. It is the student's responsibility to manage their time properly. As an alumni of the cheer team, I don't believe that all of the cheerleaders act like mean girls. I agree that a whole group of cheerleaders can be perceived as intimidating (all thanks to stereotypes) but these assumptions are false. Stereotypes say that cheerleaders are selfish and stupid. Majority of the cheerleaders have high GPAs and take AP and Honors classes. Class of 2013's top 3 students consisted of 2 cheerleaders. A lot of the cheerleaders participate in volunteer work and ASB as well. Ms. Angelica is AMAZING. She can be your best friend and give you the respect that you deserve.",
                  "reviewerRating": 5,
                  "lastUpdatedDate": "October 2023",
                  "institutionGreatschoolId": 614264,
                  "maponicsId": 1583435,
                  "url": "https://www.greatschools.org/california/san-pedro/14264-Port-Of-Los-Angeles-High-School/"
                }
              ],
              "schoolGranularRatings": [
                {
                  "schoolId": 286938,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Equity",
                  "schoolGranularRatingValue": "8",
                  "schoolGranularRatingCopyText": "Underserved students at this school are performing better than other students in the state, and this school is likely closing achievement gaps between different student groups.",
                  "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                },
                {
                  "schoolId": 286935,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "Test Scores",
                  "schoolGranularRatingValue": "8",
                  "schoolGranularRatingCopyText": "State test results at this school are above the state average, so students are likely performing at or above grade level.\n\nBecause state averages can be low, some students at this school may still not be performing at grade level.\n",
                  "schoolGranularRatingsDescription": "The Test Score Rating examines how students at this school performed on standardized tests compared with other schools in the state. The Test Rating was created using 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2022 California Science Test data from California Department of Education."
                },
                {
                  "schoolId": 286936,
                  "dataSourceId": 64,
                  "schoolGranularRatingType": "College Readiness",
                  "schoolGranularRatingValue": "7",
                  "schoolGranularRatingCopyText": "This school is above the state average in key measures of college and career readiness, such as graduation rates, college entrance exams, and advanced courses.\n\nEven at schools with strong college and career readiness, there may be students who are not getting the opportunities they need to succeed.\n",
                  "schoolGranularRatingsDescription": "The College Readiness Rating uses this high school's graduation rates, college entrance exam participation and performance, or AP, IB, or Dual Enrollment participation to determine how well schools are preparing students for success in college and beyond. The College Readiness Rating was created using the following data from the 2018 Civil Rights Data Collection: percentage of students enrolled in IB, AP or Dual Enrollment classes in grades 9-12, using 2019 SAT percent college ready data from California Department of Education, using 2019 Average ACT score data from California Department of Education, using 2019 ACT percent college ready data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
                }
              ],
              "schoolDistrict": {
                "id": 9940,
                "districtName": "Los Angeles Unified School District",
                "address": "333 South Beaudry Avenue",
                "city": "Los Angeles",
                "stateCode": "CA",
                "zip": "90017",
                "latitude": 34.056198,
                "longitude": -118.257172,
                "websiteUrl": "http://www.lausd.net",
                "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
              },
              "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/san-pedro/14264-Port-Of-Los-Angeles-High-School/#Reviews",
              "elementary": false,
              "middle": false,
              "high": true,
              "lastUpdatedDate": "November 2023",
              "hasMultipleCatchmentAreas": false
            }
          ],
          "hasChoiceSchools": true,
          "districtName": "Los Angeles Unified School District",
          "districtUrl": "http://www.lausd.net",
          "districtId": 9940
        }
      ],
      "schoolsToShowOnDP": [
        {
          "servesHome": true,
          "greatSchoolsRating": 6,
          "parentRating": 4,
          "distanceInMiles": "0.1",
          "gradeRanges": "K-5",
          "institutionType": "Public",
          "name": "Gardner Street Elementary School",
          "schoolUrl": "/school/186026/CA/Los-Angeles/Gardner-Street-Elementary-School",
          "searchUrl": "/school/186026/CA/Los-Angeles/Gardner-Street-Elementary-School",
          "greatSchoolOverviewUrl": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/",
          "id": 186026,
          "numberOfStudents": 408,
          "fullAddress": "7450 Hawthorn Ave, Los Angeles, CA 90046",
          "numReviews": 61,
          "studentToTeacherRatio": 25,
          "websiteUrl": "http://www.gardnerstreetschool.org",
          "schoolReviews": [
            {
              "schoolId": 39556114,
              "reviewedBy": "Parent",
              "datePosted": "August 2022",
              "review": "Principal and staff are ignorant. My child learned nothing",
              "reviewerRating": 1,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 5570031,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556113,
              "reviewedBy": "Parent",
              "datePosted": "August 2022",
              "review": "Amazing school! My son went to Gardner street school from pre-K to 5th grade and was the best; great teachers, very involved and committed. Great sense of community.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 5547784,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556112,
              "reviewedBy": "Parent",
              "datePosted": "February 2020",
              "review": "My husband and I assumed that we will have to go Private for our son's Elementary education, because of the urban myth of \"there are no good public schools in Hollywood\". As we were touring all the private schools in the area, we decided to stop by the Open House at our zoned public school, Gardner - just to see for ourselves how much worse it would be. We were shocked to find that we loved everything about Gardner! The way it looks and feels (happy! Filled with art, music and laughter), and mostly its progressive mindset. We were so shaken up that we went back two more times to confirm that our initial impression was not skewed in some way... Our son is now a thriving 2nd grader at Gardner, and we truly do love this school.Gardner Street Elementary is a neighborhood gem with an engaged principal, excellent teachers and helpful staff, and a small but very active parent body that makes things happen. Many students live outside the school zone but they and their parents like Gardner so much that they've chosen to permit in.What makes Gardner so special are its diverse staff and students, family-like atmosphere, and the many \"extras\". Thanks to annual fundraisers coupled with lots of fun events for kids and families throughout the school year, \"non-essential\" classes that are not paid for by LAUSD and not offered in many public schools - such as Visual Art, Music, PE, Yoga, Organic Edible Garden, and (new!) Science Lab - are part of the everyday curriculum and available to every child at Gardner. There are many really great afterschool programs to choose from, and occasional workshops for parents/caregivers. Campus beautification is ongoing.Besides all the fun, there is serious learning going on as well. Learning to truly *understand* and not just memorize to ace tests. So much learning goes on at school that - as supported by countless studies and best-performing schools in Finland - there are no homework requirements for the younger kids.Most importantly, my child has developed love for reading and learning, he is excited about going to school every day, and he feels safe there.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 4418512,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556111,
              "reviewedBy": "Parent",
              "datePosted": "September 2019",
              "review": "We just love Gardner and couldn't have had a better experience. The emphasis on strong values is just amazing and all the kids are so kind! We've found the teachers and admins to be really open to differently-abled kids and their needs. We feel very lucky to have found Gardner!",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 4226639,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556110,
              "reviewedBy": "Parent",
              "datePosted": "September 2019",
              "review": "Originally, we were planning on going to private school then we checked out Gardner and it has turned out awesome.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 4225875,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556109,
              "reviewedBy": "Parent",
              "datePosted": "May 2019",
              "review": "We chose to go to Gardner as a transfer family. When we walked in it felt different than other schools. It was clean, people were friendly and it felt HAPPY! There were happy children around everywhere. It was bright and colorful and pleasant to be in. We have had good teachers, met wonderful parents, and became very involved. The school has a great group of parents running the parent group that raises money all year for the programs that LAUSD doesn't provide. PE, Music, Art, Yoga and Edible Gardening are all only provided because the parents raise the money. And it's not a crazy amount. They aren't hounding you for money. It's usually through fun events like carnivals or dances. They encourage a monthly donation of as little as 5-10 dollars a month and it's totally doable and worth it for what the kids get in the end. Parents don't realize that other public schools don't offer those programs. But Gardner students are thriving thanks to them. We believe in public schools. And LAUSD has a pretty rough reputation, but this is one of those schools that's a diamond in the rough. It is diverse both racially and economically. It's has three main languages spoken among students. It has loving and respectful teachers and administration. Parents that are involved and creating an enriching environment for kids and helping to create a healthier education for them. It's a great size and very inclusive and we are THRILLED to be a part of it.",
              "reviewerRating": 5,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 4061327,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            },
            {
              "schoolId": 39556108,
              "reviewedBy": "Parent",
              "datePosted": "May 2019",
              "review": "I’m running from this school since we day one. Schools are what you make of it but no ..... unless you pay them for alllll the donations and your attending each bake sale they have an entitled , nasty , belittling attitude. There is no diversity at least with African American or mixed race . They will accent your child but they definitely have the attitude of go else where why are you here.",
              "reviewerRating": 1,
              "lastUpdatedDate": "October 2023",
              "institutionGreatschoolId": 602098,
              "maponicsId": 4056201,
              "url": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/"
            }
          ],
          "schoolGranularRatings": [
            {
              "schoolId": 285686,
              "dataSourceId": 64,
              "schoolGranularRatingType": "Test Scores",
              "schoolGranularRatingValue": "8",
              "schoolGranularRatingCopyText": "State test results at this school are above the state average, so students are likely performing at or above grade level.\n\nBecause state averages can be low, some students at this school may still not be performing at grade level.\n",
              "schoolGranularRatingsDescription": "The Test Score Rating examines how students at this school performed on standardized tests compared with other schools in the state. The Test Rating was created using 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2022 California Science Test data from California Department of Education."
            },
            {
              "schoolId": 285685,
              "dataSourceId": 64,
              "schoolGranularRatingType": "Equity",
              "schoolGranularRatingValue": "5",
              "schoolGranularRatingCopyText": "Underserved students at this school are performing about as well as other students in the state, but this school may still have achievement gaps between different student groups.",
              "schoolGranularRatingsDescription": "Using the state’s Department of Education data, the Equity Overview Rating compares this school to other schools in the state by looking at how well this school serves the needs of disadvantaged students relative to all students using state proficiency tests, student growth or academic progress data, and college readiness data. The Equity Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, using 2022 4-year high school graduation rate data from California Department of Education, using 2022 Percent of students who meet UC/CSU entrance requirements data from California Department of Education, and using 2019 demographic data from California Department of Education."
            },
            {
              "schoolId": 285687,
              "dataSourceId": 64,
              "schoolGranularRatingType": "Academic Progress",
              "schoolGranularRatingValue": "6",
              "schoolGranularRatingCopyText": "Students at this school are making average academic progress from one grade to the next compared to similar students in the state.",
              "schoolGranularRatingsDescription": "The Academic Progress Rating is a growth proxy rating based on a model using unmatched cohorts, or school-level data instead of student-level data. The Academic Progress Rating was created using 2019 and 2022 California Science Test data from California Department of Education, using 2019 and 2022 California Assessment of Student Performance and Progress (CAASPP) data from California Department of Education, and using 2019 demographic data from California Department of Education."
            }
          ],
          "schoolDistrict": {
            "id": 9940,
            "districtName": "Los Angeles Unified School District",
            "address": "333 South Beaudry Avenue",
            "city": "Los Angeles",
            "stateCode": "CA",
            "zip": "90017",
            "latitude": 34.056198,
            "longitude": -118.257172,
            "websiteUrl": "http://www.lausd.net",
            "greatschoolDistrictOverviewUrl": "https://www.greatschools.org/california/los-angeles/los-angeles-unified-school-district/"
          },
          "greatschoolParentReviewsUrl": "https://www.greatschools.org/california/los-angeles/2098-Gardner-Street-Elementary-School/#Reviews",
          "elementary": true,
          "middle": false,
          "high": false,
          "lastUpdatedDate": "October 2023",
          "hasMultipleCatchmentAreas": false
        }
      ],
      "totalSchoolsServiced": 0,
      "sectionPreviewText": "Average rating 4.9 out of 10",
      "shouldHideSection": false,
      "hasChoiceDistricts": true
    },
    "mlsDisclaimerInfo": {
      "showDisclaimerWithMlsInfo": false,
      "showDisclaimerInFooter": true,
      "listingBrokerName": "Compass",
      "mlsDisclaimer": "<img src=\"${IMAGE_SERVER}/images/logos/claw_small.png\" />Based on information from The MLS CLAW as of (date the MLS data was obtained). All data, including all measurements and calculations of area, is obtained from various sources and has not been, and will not be, verified by broker or MLS. All information should be independently reviewed and verified for accuracy. Properties may or may not be listed by the office/agent presenting the information.",
      "mlsDislcaimerPlainText": "Based on information from CARETS as of (today's date). The information being provided by CARETS is for the visitor's personal, noncommercial use and may not be used for any purpose other than to identify prospective properties visitor may be interested in purchasing. The data contained herein is copyrighted by CARETS, CLAW, CRISNet MLS, i-Tech MLS, PSRMLS and/or VCRDS and is protected by all applicable copyright laws. Any dissemination of this information is in violation of copyright laws and is strictly prohibited. Any property information referenced on this website comes from the Internet Data Exchange (IDX) program of CARETS. All data, including all measurements and calculations of area, is obtained from various sources and has not been, and will not be, verified by broker or MLS. All information should be independently reviewed and verified for accuracy. Properties may or may not be listed by the office/agent presenting the information.",
      "lastUpdatedString": "May 2, 2024 11:57 AM",
      "logoImageFileName": "claw_small.png",
      "listingBrokerNumber": "310-652-6285",
      "listingAgentName": "Daniel Jacobson",
      "listingAgentNumber": "323-346-9111"
    },
    "zoningDataInfo": {
      "zoneName": "Residential",
      "zoneType": {
        "zoneType": "Residential",
        "zoneSubType": "Multi Family",
        "display": [
          "Residential Multi Family"
        ]
      },
      "zoneCode": "R1-1-HPOZ",
      "permittedLandUse": [
        "Single-Family",
        "Two-Family",
        "Multi-Family",
        "ADU",
        "Commercial"
      ],
      "notPermittedLandUse": [
        "Short-Term Rentals",
        "Industrial"
      ],
      "asOfRightLandUse": [
        "One family dwellings",
        "Name plates and signs",
        "Parks playgrounds or community centers owned and operated by a governmental agency",
        "Truck gardening the keeping of equines poultry rabbits and chinchillas in conjunction with the residential use of the lot provided such animal keeping is not for commercial purposes the keeping of equines shall be permitted only on lots having an area of 20000 square feet or more where equines are being kept the number of such animals being kept shall not exceed one for each 5000 square feet of lot area",
        "Two family dwellings on lots having a side lot line adjoining a lot in a commercial or industrial zone provided the lot on which the use is located does not extend more than 65 feet from the boundary of the less restrictive zone which it adjoins and there is a minimum lot area of 5000 square feet for each two family dwelling",
        "Accessory buildings including private garages accessory living quarters servants quarters recreation rooms or private stables provided no accessory living quarters nor servants quarters are located or maintained on a lot having an area less than 10000 square feet no stable is located or maintained on a lot having an area of less than 20000 square feet and its capacity does not exceed one equine for each 5000 square feet of lot area an accessory living quarters servants quarters recreation room or private garage or any combination of said uses may be included in one building not exceeding two stories in height automobile parking space is required in connection with permitted uses and additional space may be provided in accordance with the provisions of Sec. 12.21 A."
      ],
      "conditionalLandUse": [
        "Airports or heliports in connection with an airport",
        "Auditoriums stadiums arenas and the like",
        "Correctional or penal institutions",
        "Educational institutions",
        "Electric power generating sites plants or stations fueled by any thermal power source or technology provided that the facilities comply with all applicable state and federal regulations",
        "Golf courses and facilities properly incidental to that use",
        "Chipping or grinding facility",
        "Composting facility",
        "Curing facility",
        "Mulching facility",
        "Accessory uses and home occupations subject to the (conditions) specified in Section 12.05 A.16. of this code"
      ],
      "accessoryLandUse": [
        "Backyard beekeeping as an accessory use"
      ],
      "localZoningCodeUrl": "https://codelibrary.amlegal.com/codes/los_angeles/latest/lamc/0-0-0-108121#JD_C1A2",
      "zoningFullReportUrl": "https://zoneomics.com/zoning_report?lat=34.0995491&lng=-118.3542357&resource=redfin",
      "lastUpdatedDateString": "Last updated Apr 30, 2024",
      "message": "Success."
    }
  }
}
```

## Contact
Please visit us through [epctex.com](https://epctex.com) to see all the products that are available for you. If you are looking for any custom integration or so, please reach out to us through the chat box in [epctex.com](https://epctex.com). In need of support? [devops@epctex.com](mailto:devops@epctex.com) is at your service.
