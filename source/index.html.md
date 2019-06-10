---
title: OnVoard API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://onvoard.com'>OnVoard Home</a>
  - <a href='mailto:support@onvoard.com'>Contact Us</a>
  - <a href='https://github.com/lord/slate'>Powered by Slate</a>

search: true
---

# Introduction
You can use OnVoard's API to access various endpoints, and perform tasks in programmatic fashion such as sending email surveys via triggers. Currently, the key use cases supported by OnVoard's API are:

* Query reviews
* Send review requests
* Send interview requests
* Send surveys

# Authentication

```shell
# Api calls must be made with token header
curl 'https://api.onvoard.com/v1/reviews' \
-H 'Authorization: Token {API_KEY}'
```

OnVoard currently use api key for authentication. Go to [api details](https://console.onvoard.com/api-details) page to get your api key.
Add `Authorization: Token {API_KEY}` header to your api request for authentication.

<aside class="notice">
Currently only owner has access to api keys.
</aside>

# Pagination

```shell
# Return up to 50 records for page 3.
https://api.onvoard.com/v1/reviews?page=3&page_size=50
```

The default page size for paginated queries is **10**. You can increase the page size to return up to **100** records by specifying the `page_size` parameter in your GET request. You can traverse to specific page with the `page` parameter. Index for page starts from **1**.

# Errors

Any request that did not succeed will return a `4xx` or `5xx` error. `4xx` range errors means that there's a problem with the request, such as missing parameter. `5xx` range means that something went wrong on our end, such as server failure.

# Reviews
Represents a review object. Reviews can be collected from various sources like google and facebook.

## Review Object

```json
{
  "id": "rvw_7vomjp0vjokbq09",
  "active": true,
  "account": {
    "id": "acct_u4o2a8hmqk5rzp9",
    "name": "Carousell"
  },
  "status": "PENDING",
  "recommend": null,
  "rating": 5.0,
  "title": "Thank you, Carousell",
  "content": "I absolutely love this app!",
  "url": "",
  "source_review_id": "3465825614",
  "review_date": "2018-11-27",
  "reviewer_name": "Rodristar",
  "reviewer_pic_url": "",
  "reply_text": "",
  "reply_date": null,
  "attributes": [{
    "name": "App Version",
    "value": "2.91.0"
  }],
  "score": "FIVE",
  "entity": {
    "id": "ent_viyquhuf85f7qlw",
    "name": "Carousell"
  },
  "source": {
    "id": "appstore",
    "name": "iOS App Store"
  },
  "created_timestamp": 1543414453
}
```

### Attributes
Field | Type | Description
--------- | ------- | -----------
id | string | Unique identifier for the object.
active | boolean | Active state of review. Filtered reviews will return `false`.
account.id | string | Unique identifier of linked account.
account.name | string | Name of linked account.
status | enum ([Review Status](#review-status)) | Status of this review.
recommend | boolean | Recommend score for reviewed item. Field will be null if binary rating system is not used.
rating | float | Rating score for reviewed item. Field will be null if 5-star rating system is not used.
title | string | Title of review.
content | string | Content of review.
url | string | Url to review.
source_review_id | string | Review's unique identifier in source's context.
review_date | string | Date of review. ISO 8601 format, `YYYY-MM-DD`.
reviewer_name | string | Reviewer's name.
reviewer_pic_url | string | Reviewer's pic url.
reply_text | string | Replied text.
reply_date | string | Replied date. ISO 8601 format, `YYYY-MM-DD`.
attributes[].name | string | Name for review's attribute.
attributes[].value | string | Value for review's attribute.
score | enum ([Review Score](#review-score)) | Score for this review.
entity.id | string | Unique identifier of linked entity.
entity.name | string | Name of linked entity.
source.id | string | Unique identifier of linked source.
source.name | string | Name of linked source.
created_timestamp | integer | Timestamp when review was added to our system.

## Types
List of available enum types under this resource.

### Review Status
Value | Description
--------- | -------
PENDING | Pending Response
RESPONDED | Responded
SKIP | Skip Response

### Review Score
Value | Description
--------- | -------
NA | No Scores
ZERO | 0 Star Range
ONE | 1 Star Range
TWO | 2 Star Range
THREE | 3 Star Range
FOUR | 4 Star Range
FIVE | 5 Star Range
POSITIVE | Recommend
NEGATIVE | Doesn't Recommend


## Retrieve a Review

> Example Request

```shell
curl 'https://api.onvoard.com/v1/reviews/rvw_7vomjp0vjokbq09' \
-H 'Authorization: Bearer {API_KEY}'
```

> Example Response

```json
{
  "id": "rvw_7vomjp0vjokbq09",
  "active": true,
  "account": {
    "id": "acct_u4o2a8hmqk5rzp9",
    "name": "Carousell"
  },
  "status": "PENDING",
  "recommend": null,
  "rating": 5.0,
  "title": "Thank you, Carousell",
  "content": "I absolutely love this app!",
  "url": "",
  "source_review_id": "3465825614",
  "review_date": "2018-11-27",
  "reviewer_name": "Rodristar",
  "reviewer_pic_url": "",
  "reply_text": "",
  "reply_date": null,
  "attributes": [{
    "name": "App Version",
    "value": "2.91.0"
  }],
  "score": "FIVE",
  "entity": {
    "id": "ent_viyquhuf85f7qlw",
    "name": "Carousell"
  },
  "source": {
    "id": "appstore",
    "name": "iOS App Store"
  },
  "created_timestamp": 1543414453
}
```

Retrieve review with given ID.

### HTTP Request
`GET https://api.onvoard.com/v1/reviews/:review_id`

### Returns
Review object if valid ID was provided.


## Patch a Review

> Example Request

```shell
curl 'https://api.onvoard.com/v1/reviews/rvw_7vomjp0vjokbq09' \
-H 'Authorization: Bearer {API_KEY}'
-d status="RESPONDED"
```

> Example Response

```json
{
  "id": "rvw_7vomjp0vjokbq09",
  "active": true,
  "account": {
    "id": "acct_u4o2a8hmqk5rzp9",
    "name": "Carousell"
  },
  "status": "RESPONDED",
  "recommend": null,
  "rating": 5.0,
  "title": "Thank you, Carousell",
  "content": "I absolutely love this app!",
  "url": "",
  "source_review_id": "3465825614",
  "review_date": "2018-11-27",
  "reviewer_name": "Rodristar",
  "reviewer_pic_url": "",
  "reply_text": "",
  "reply_date": null,
  "attributes": [{
    "name": "App Version",
    "value": "2.91.0"
  }],
  "score": "FIVE",
  "entity": {
    "id": "ent_viyquhuf85f7qlw",
    "name": "Carousell"
  },
  "source": {
    "id": "appstore",
    "name": "iOS App Store"
  },
  "created_timestamp": 1543414453
}
```

Patch review with given ID.

### HTTP Request
`PATCH https://api.onvoard.com/v1/reviews/:review_id`

### Returns
Modified review object if patch succeeded.

### Arguments
Field | Type | Required | Description
--------- | ------- | ----------- | -----------
active | boolean | No | Active state of review.
status | enum ([Review Status](#review-status)) | No | Status of review.


## List all Reviews

> Example Request

```shell
curl 'https://api.onvoard.com/v1/reviews' \
-H 'Authorization: Bearer {API_KEY}'
```

> Example Response

```json
{
  "num_records": 1,
  "num_pages": 1,
  "current_page": 1,
  "results": [{
    "id": "rvw_7vomjp0vjokbq09",
    "active": true,
    "account": {
      "id": "acct_u4o2a8hmqk5rzp9",
      "name": "Carousell"
    },
    "status": "PENDING",
    "recommend": null,
    "rating": 5.0,
    "title": "Thank you, Carousell",
    "content": "I absolutely love this app!",
    "url": "",
    "source_review_id": "3465825614",
    "review_date": "2018-11-27",
    "reviewer_name": "Rodristar",
    "reviewer_pic_url": "",
    "reply_text": "",
    "reply_date": null,
    "attributes": [{
      "name": "App Version",
      "value": "2.91.0"
    }],
    "score": "FIVE",
    "entity": {
      "id": "ent_viyquhuf85f7qlw",
      "name": "Carousell"
    },
    "source": {
      "id": "appstore",
      "name": "iOS App Store"
    },
    "created_timestamp": 1543414453
  }]
}
```

List all reviews user has access to.

### HTTP Request
`GET https://api.onvoard.com/v1/reviews`

### Returns
List of review objects.

### Arguments
Field | Type | Description
--------- | ------- | -----------
active | boolean | Use `true` if you only want to return unfiltered reviews and `false` to return filtered reviews.
status | enum ([Review Status](#review-status)) | Filter by review status.
keyword | string | Filter by specific keyword.
scores | string | Comma-seperated list of scores to filter with. See [Review Score](#review-score).
created_before | integer | Filter reviews created before provided timestamp.
created_after | integer | Filter reviews created after provided timestamp.
page | integer | Traverse to specific page. Page index starts from `1`.
page_size | integer | Number of records you want to return for each page. Can be from `1` to `100`. Default is `10`.


# Review Requests
Represents a review request object. Must be created before we can send review request to email/phone.

## Review Request Object

```json
{
  "id": "rr_qgn3iucff6gflng",
  "name": "Urger Burgers",
  "active": true,
  "template": {
      "id": "rr_tpl_frdaeg2f86pu248",
      "name": "Urger Burgers"
  },
  "account": {
      "id": "acct_u4o2a8hmqk5rzp9",
      "name": "Urger Burgers"
  },
  "preferred_distribution": "SMS",
  "feedback_domain": null,
  "feedback_slug": "",
  "kiosk_domain": null,
  "kiosk_slug": "",
  "links": [{
      "id": "lnk_y8k49gsxmfkm23k"
  }],
  "default_feedback_url": "https://onvoard.io/feedback/rr_qgn3iucff6gflng",
  "custom_feedback_url": "",
  "default_kiosk_url": "https://onvoard.io/kiosk/rr_qgn3iucff6gflng",
  "custom_kiosk_url": "",
  "eligible_sms": false,
  "eligible_email": true
}
```

### Attributes
Field | Type | Description
--------- | ------- | -----------
id | string | Unique identifier for the object.
name | string | Name for review request.
active | boolean | Active state of review request. Archived review request will be `false`.
template.id | string | Unique identifier of linked template.
template.name | string | Name of linked template.
account.id | string | Unique identifier of linked account.
account.name | string | Name of linked account.
preferred_distribution | enum ([Review Request Distribution](#review-request-distribution)) | Preferred distribution for review request.
feedback_domain.id | string | Unique identifier of linked custom domain.
feedback_domain.fqdn | string | Fully qualified domain name of linked custom domain. Will be something like `feedback.yourdomain.com`.
kiosk_domain.id | string | Unique identifier of linked custom domain.
kiosk_domain.fqdn | string | Fully qualified domain name of linked custom domain. Will be something like `kiosk.yourdomain.com`.
links[].id | string | Unique identifier of relevant link.
default_feedback_url | string | Default feedback url generated by our system.
custom_feedback_url | string | Custom feedback url generated based on the concatenation of `feedback_domain` and `feedback_slug`.
default_kiosk_url | string | Default kiosk url generated by our system.
custom_kiosk_url | string | Custom kiosk url generated based on the concatenation of `feedback_domain` and `feedback_slug`.
eligible_sms | boolean | If review request is eligible to be sent via sms.
eligible_email | boolean | If review request is eligible to be sent via email.


## Types
List of available enum types under this resource.

### Review Request Distribution
Value | Description
--------- | -------
SMS | Distribute review request via sms
EMAIL | Distribute review request via email


## Retrieve a Review Request

> Example Request

```shell
curl 'https://api.onvoard.com/v1/review-requests/rr_qgn3iucff6gflng' \
-H 'Authorization: Bearer {API_KEY}'
```

> Example Response

```json
{
  "id": "rr_8net8goc2fupqks",
  "name": "Bugis Village",
  "active": true,
  "template": {
    "id": "rr_tpl_jzdrqmkuciygdlg",
    "name": "Default"
  },
  "account": {
    "id": "acct_u4o2a8hmqk5rzp9",
    "name": "Urger Burgers"
  },
  "preferred_distribution": "SMS",
  "feedback_domain": null,
  "feedback_slug": "",
  "kiosk_domain": null,
  "kiosk_slug": "",
  "links": [{
    "id": "lnk_8y8kfmtkp5tlpta"
  }, {
    "id": "lnk_myqr1vxp56cl1r9"
  }],
  "default_feedback_url": "https://onvoard.io/feedback/rr_8net8goc2fupqks",
  "custom_feedback_url": "",
  "default_kiosk_url": "https://onvoard.io/kiosk/rr_8net8goc2fupqks",
  "custom_kiosk_url": "",
  "eligible_sms": true,
  "eligible_email": true
}
```

Retrieve review request with given ID.

### HTTP Request
`GET https://api.onvoard.com/v1/review-requests/:review_request_id`

### Returns
Review request object if valid ID was provided.


## Send a Review Request

> Example Request

```shell
curl 'https://api.onvoard.com/v1/review-requests/rr_qgn3iucff6gflng/send' \
-H 'Authorization: Bearer {API_KEY}'
-d use_automation_settings=true \
-d name="John" \
-d email="johndoe@gmail.com"
```

> Example Response

```json
{
  "id": "rcpt_fvslwlgot1cl3w2bu3u13pggz",
  "account": {
    "id": "acct_u4o2a8hmqk5rzp9",
    "name": "Urger Burgers"
  },
  "review_request": {
    "id": "rr_8net8goc2fupqks",
    "name": "Bugis Village"
  },
  "distribution": "EMAIL",
  "name": "John",
  "phone": "",
  "email": "johndoe@gmail.com",
  "clicked": false,
  "status": "QUEUED",
  "error_msg": "",
  "lag_window": null,
  "sending_timestamp": null,
  "delivered_timestamp": null
}
```

Send review request with given ID.

### HTTP Request
`POST https://api.onvoard.com/v1/review-requests/:review_request_id/send`

### Returns
[Recipient](#recipient-object) object if review request was sent.


### Arguments
Field | Type | Required | Description
--------- | ------- | ----------- | -----------
use_automation_settings | boolean | Yes | If `true` we will use preconfigured automation settings for deduplication and scheduling. If `false`, we won't do any deduplication and will send out review request immediately. For most cases, we recommend using `true`.
lag_window | integer | No | Delayed number of minutes before after sending out review request. If specified, lag window must be between 5 and 1440 (1 day).
name | string | No | How should we address your recipient? We recommend using the first name or initials of recipient.
email | string | No | Provide this if you want to send via email. **Either email or phone must be provided**.
phone | string | No | Full phone number inclusive of country code and area code (if there are any). Provide this if you want to send via sms. **Either email or phone must be provided**.


## List all Review Requests

> Example Request

```shell
curl 'https://api.onvoard.com/v1/review-requests' \
-H 'Authorization: Bearer {API_KEY}'
```

> Example Response

```json
{
  "num_records": 1,
  "num_pages": 1,
  "current_page": 1,
  "results": [{
    "id": "rr_8net8goc2fupqks",
    "name": "Bugis Village",
    "active": true,
    "template": {
      "id": "rr_tpl_jzdrqmkuciygdlg",
      "name": "Default"
    },
    "account": {
      "id": "acct_u4o2a8hmqk5rzp9",
      "name": "Urger Burgers"
    },
    "preferred_distribution": "SMS",
    "feedback_domain": null,
    "feedback_slug": "",
    "kiosk_domain": null,
    "kiosk_slug": "",
    "links": [{
      "id": "lnk_8y8kfmtkp5tlpta"
    }, {
      "id": "lnk_myqr1vxp56cl1r9"
    }],
    "default_feedback_url": "https://onvoard.io/feedback/rr_8net8goc2fupqks",
    "custom_feedback_url": "",
    "default_kiosk_url": "https://onvoard.io/kiosk/rr_8net8goc2fupqks",
    "custom_kiosk_url": "",
    "eligible_sms": true,
    "eligible_email": true
  }]
}
```

List all review requests user has access to.

### HTTP Request
`GET https://api.onvoard.com/v1/review-requests`

### Returns
List of review request objects.

### Arguments
Field | Type | Description
--------- | ------- | -----------
active | boolean | Use `true` if you only want to return active review requests and `false` to return archived review requests.
keyword | string | Filter by specific keyword.
page | integer | Traverse to specific page. Page index starts from `1`.
page_size | integer | Number of records you want to return for each page. Can be from `1` to `100`. Default is `10`.


# Recipients
A recipient of review request. This is created upon sending of review request.

## Recipient Object

```json
{
  "id": "rr_8net8goc2fupqks",
  "name": "Bugis Village",
  "active": true,
  "template": {
    "id": "rr_tpl_jzdrqmkuciygdlg",
    "name": "Default"
  },
  "account": {
    "id": "acct_u4o2a8hmqk5rzp9",
    "name": "Urger Burgers"
  },
  "preferred_distribution": "SMS",
  "feedback_domain": null,
  "feedback_slug": "",
  "kiosk_domain": null,
  "kiosk_slug": "",
  "links": [{
    "id": "lnk_8y8kfmtkp5tlpta"
  }, {
    "id": "lnk_myqr1vxp56cl1r9"
  }],
  "default_feedback_url": "https://onvoard.io/feedback/rr_8net8goc2fupqks",
  "custom_feedback_url": "",
  "default_kiosk_url": "https://onvoard.io/kiosk/rr_8net8goc2fupqks",
  "custom_kiosk_url": "",
  "eligible_sms": true,
  "eligible_email": true
}
```

### Attributes
Field | Type | Description
--------- | ------- | -----------
id | string | Unique identifier for the object.
account.id | string | Unique identifier of linked account.
account.name | string | Name of linked account.
review_request.id | string | Unique identifier of linked review request.
review_request.name | string | Name of linked review request.
distribution | enum ([Review Request Distribution](#review-request-distribution)) | How will review request be distributed to recipient.
name | string | This may not always be the full name of recipient. Provided value can be the first name or initials of recipient.
phone | string | Full phone number of recipient inclusive of country and area code.
email | string | Email of recipient.
clicked | boolean | If recipient clicked on review request link to feedback page.
status | enum ([Recipient Status](#recipient-status)) | Status of recipient.
error_msg | string | Details of error. Will be provided if `status` is `FAILED`.
lag_window | integer | Number of minutes delayed before review request was sent to recipient
sending_timestamp | integer | Timestamp when send event was invoked
delivered_timestamp | integer | Timestamp when review request was delivered to recipient


## Types
List of available enum types under this resource.

### Recipient Status
Value | Description
--------- | -------
QUEUED | Recipient is queued and waiting to be sent
FAILED | Encountered failure when we tried to send to recipient
SENDING | Currently sending to recipient
DELIVERED | Successfully sent to recipient
