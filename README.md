# AccommodationSurvey

## Foundations

This is based on the [Heroku HTTP API Design Guide](https://github.com/interagent/http-api-design) and the [18F API Standards](https://github.com/18F/api-standards).

The API will be REST based and only provide a single POST message.

The API will be versioned and the version number will be included as first parameter in the URL, e.g. `api.stats.govt.nz/api/v1/survey`.

Authentication will be done through the use of API keys. There will be one API key for each PMS provider.

No caching required.

No pagination required.

The character set will be UTF-8 for everything.

Times will be specified as per [ISO 8601 standard](http://en.wikipedia.org/wiki/ISO_8601).

### 'POST /survey'

#### Parameters

```
survey-type     The type of survey this request is for, e.g. `Accommodation`
api-key         The Api key of the PMS provider/intermediary
request-id      A client set correlation id that will be returned with each response and can help with debugging.
```

These initial parameters need to be provided as GET parameters in the URL. The reason to separate these from the survey payload is the future option to re-use this endpoint for other suryeys.

`api.stats.govt.nz/api/v1/survey?survey-type=Accommodation&api-key=123456789abcdef&request-id=123xyz`

The actual survey data will be sent as structured JSON in the POST request body. The API will allow for one data set of a single accommodation provider to be submitted in each request.

```
{  
  "statistics-id":"AB1234567",
  "year":2014,
  "month":11,
  "total-stay-unit-nights":90,
  "total-nz-guests":60,
  "total-overseas-guests":50,
  "total-unknown-residence":10,
  "total-guest-nights":120,
  "total-guest-arrivals":85,
  "number-of-stay-units":8
}
```

#### Response

Status-codes ([reference](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)):
* `201 Created`: Request successful
* `400 Bad request`: The request could not be understood by the server
* `401 Unauthorised`: Request failed because user is not authenticated
* `403 Forbidden`: Request failed because user does not have authorization to access a specific resource
* `429 Too Many Requests`: You have been rate-limited, retry later
* `500 Internal Server Error`: Something went wrong on the server, check status site and/or report the issue

Successful request
```
{
  "status": "Ok",
  "timestamp": "2014-11-25T11:08:21+12:00",
  "request-id": "01234567-89ab-cdef-0123-456789abcdef"
}
```

Erroneous request
```
{
  "status": "Error",
  "error-id": "234",
  "error-message": "Invalid provider-id, please provide a valid provider-id",
  "case-number": "2354234",
  "support-email": "api-support@stats.govt.nz",
  "support-phone": "+64-(4)-123 456 789",
  "timestamp": "2014-11-25T11:08:21+12:00",
  "request-id": "01234567-89ab-cdef-0123-456789abcdef"
}
```

### Security

The API will use TLS for encryption and will only have an HTTPS endpoint (no endpoint on port 80).

