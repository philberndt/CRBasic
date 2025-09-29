# Header

A string that indicates the additional header information to include in the request. As a result of the request, the server may return header information that is stored in the string. The Header parameter can be an empty string if no additional header information is required.

In the case that more than one header is required, headers can be separated by & CHR(13) & CHR(10) &. For example:

HTTPPost(https://api.placeholder.com/v1/chat/completions,Query\_str,Response,"Content-Type: application/json"**& CHR(13) & CHR(10) &**"Authorization: Bearer xx-XXXXXX")

Type: String or variable formatted as a string

- Text (description)
