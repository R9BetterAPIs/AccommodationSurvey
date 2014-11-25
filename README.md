# AccomodationSurvey

## Messages

The API will be REST based and only provide a single POST message.

Versioning will be included as first parameter in the URL, e.g. 'api.stats.govt.nz/api/v1/survey'.

Authentication will be done through the use of API keys. There will be one API key for each PMS provider.

### 'POST /survey'

#### Parameters

```
survey-typ      The type of survey this request is for, e.g. `Accomodation`
api-key         The Api key of the PMS provider
provider-id     ID of PMS provider
request-id      A client set correlation id that will be returned with each response and can help with debugging.
```

The actual survey data will be sent as strutured JSON in the POST request body.
```
{
  "provider": {
    "provider-id": "123456",
    "survey-data": {
      "year": "2014",
      "month": "11",
      "total-stay-unit-nights": "90",
      "total-nz-guests": "60",
      "total-overseas-guests": "50",
      "total-unknown-residence": "10",
      "total-guest-nights": "120",
      "total-guest-arrivals": "85",
      "number-of-stay-units": "8"
    }
  }
}

```

#### Response

Status-codes:
* `201`: Request successful
* `401 Unauthorized`: Request failed because user is not authenticated
* `403 Forbidden`: Request failed because user does not have authorization to access a specific resource
* `429 Too Many Requests`: You have been rate-limited, retry later
* `500 Internal Server Error`: Something went wrong on the server, check status site and/or report the issue

Successful submission
```
{
  "status": "Ok",
  "": "",
  "": "",
  "": "",
  "": "",
  "": "",
  "": "",
  "": "",
  "timestamp": "2014-11-25T12:00:00Z",
  "request-id": "01234567-89ab-cdef-0123-456789abcdef"
}
```

Error
```
{
  "status": "Error",
  "error-id": "234",
  "error-message": "Invalid provider-id, please provide a valid provider-id",
  "case-number": "2354234",
  "support-contact": "api-support@stats.govt.nz or +64-(4)-123 456 789",
  "timestamp": "2014-11-25T12:00:00Z",
  "request-id": "01234567-89ab-cdef-0123-456789abcdef"
}
```

### Security

The API will use TLS for encryption and will only have an HTTPS endpoint (no endpoint on port 80)
