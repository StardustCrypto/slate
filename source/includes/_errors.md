# Errors

<aside class="notice">
Make sure to take a look at the `message` parameter of the response. These error codes are baselines for all requests.
</aside>

The Stardust API uses the following error codes:


Error Code | Meaning
---------- | -------
200 | OK -- Everything worked OK.
400 | Request Error -- Check the `message` parameter in the response.
404 | Not Found -- The specified endpoint could not be found, are you using the right endpoint?
500 | What on earth? -- You just broke something, please contact us in the Stardust chatrooms.
