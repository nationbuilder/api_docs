API Authentication Quick-Start Guide
====================================

Preliminary: If you already have an account, just login to the control panel and
continue below. Otherwise, email [api@nationbuilder.com](mailto:api@nationbuilder.com) to request a free account,
and specify your desired slug, joeyforprez, for example. This slug will be used in your
NationBuilder URL, joeyforprez.nationbuilder.com. This cannot be changed; we
recommend using your organization or company name. You will get a response with
within one working day.

If you want to begin testing the API from your own nation or are building an app for a single customer, you can use test tokens to get started immediately. In your nation's control panel, go to Settings > Developer > API token.

These are the steps necessary to start using the API to access resources through OAuth:

1. Register the application

    Log into your nation, navigate to Settings > Apps > New App. Register your application with a name and OAuth callback URL. After you register, make sure to record the Client ID and Client Secret codes provided here for future use.

2. Ask a nation's administrator for access

    Bring the admin to this page to request access to his nation's data:

    Note: You can receive an access token to make requests on behalf of a person with permission level 'staffer', but that access token will not allow you to use the endpoints currently documented.  In the future endpoints may be individually segregated into permission level necessary.

    ```
    https://{slug}.nationbuilder.com/oauth/authorize?response_type=code&client_id=...&redirect_uri=...
    ```

    Where the client_id and redirect_uri are the values from your app's registration. Uid is the label for client_id used in the app's registration page. {slug} is the nation's slug.

    The user will be shown a form stating that your application has requested access to their nation.

3. Record the code that comes back

    If the admin accepts your request, we will redirect him/her to your redirect_uri with a code parameter in its query string, so a request will come through like this:

    ```
    GET http://www.myapp.com/oauth_callback?code=...
    ```

    Record this code for the next step

4. Exchange the code for an access token

    Exchange that code for an access token by issuing a request for one like this:

    ```
    POST https://{slug}.nationbuilder.com/oauth/token
      client_id=...
      redirect_uri=...
      grant_type=authorization_code
      client_secret=...
      code=...
    ```

    A practical example is using the command line utility cURL like this:

    ```bash
    curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --data '{"grant_type":"authorization_code", "code":"{code}", "client_id":"{client_id}", "client_secret":"{client_secret}", "redirect_uri":"{redirect_uri}"}' https://{slug}.nationbuilder.com/oauth/token
    ```

    This request will receive a response like this:

    ```json
    {
      "access_token": "a44bdfe7972f7a83f5dc485cd3a7c7d2d09b0c60828ed24657c0b61e186ed93a",
      "token_type": "bearer",
      "scope":""
    }
    ```
        Record the access token from the response you receive to this request

5. Use the access token to get data

    With this access token you can make requests on the user's behalf. See our API endpoint documentation for full details, but as an example, this is the request you would use to get the first page of people in a nation:

    ```
    GET https://{slug}.nationbuilder.com/api/v1/people?access_token=...
    ```

    Note that the API speaks only in javascript object notation (JSON), and that 406 response code means that you need to include the Content-Type and Accept headers of your request to "application/json".

Please direct API-related questions to NationBuilder Platform Director [Arion Hardison](mailto:ahardison@nationbuilder.com)
