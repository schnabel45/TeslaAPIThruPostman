# Using the Tesla API with Postman

## Authentcation
**PLEASE READ STEPS DOCUMENTATION BEFORE ATTEMPTING AUTHENTICATION.**  The authentication sequence follows the sequence outlined [here](https://tesla-api.timdorr.com/api-basics/authentication).  Familarity with [Postman](https://www.postman.com/) is recommended.  Execute steps 1 to 4 to receive an access token that can be used with API calls.  Note that some authentication information is time sensitive so the steps should be completed in a five minute period.

### There is no support for MFA at this time.  Step 3 will fail without MFA support.

### Step 1: Request Log In Page

USER PREPARATION: Enter the email address used to access your Telsa account in the parameter named login_hint.  Enter a random string in the parameter named state.  Save these changes before clicking Send.

#### Documentation

All parameters have been defined.  Update the state and login_hint as described in USER PREPARATION.

The Pre-request Script creates the code verifier and code challenge required for the API calls.

The Tests extract hidden form variables from the response and stores them in envrionment variables for subsequent calls.

### Step 2: Obtain Authorization Code

USER PREPARATION: Enter a random string in the parameter named state.  This should be the same value used in the Request Log In Page call (step 1).  On the Body tab, enter your Tesla Log In ID for the identity key, and enter your Tesla Log In password for the credential key.   Save these changes before clicking Send.

#### Documentation

All parameters have been defined.  Update the state parameter and Body keys as described in USER PREPARATION.

The Body defines form variables sent with the call.  Some form variables are assigned from environment variables; the Tesla Log In credentials are updated as described in USER PREPARATION.

The Tests extract an authentication code and stores it in the environment variable named code.

### Step 3: Exchange Authentication Code for Bearer Token

#### Documentation

The Body defines a JSON document containing the authentication code received in step 2 and the code verifier created in step 1.

The Tests extract the access token and stores it in the environment variable named access_token.

### Step 4: Exchange Bearer Token for Access Token

USER PREPARATION: Enter the client ID and secret before clicking Send. The current client ID and secret are [available here](https://pastebin.com/pS7Z6yyP).

#### Documentation

The Authorization is set to Bearer Token and Token is assigned from the environment variable access_token.

The Body defines a JSON document for the call.  Update the client ID and secret as described in USER PREPARATION.

The Tests extract access and refresh tokens.  The access token is stored in the environment variable named teslaBearerToken.  The refresh token is stored in the environment variable named teslaRefreshToken.


## Using the APIs

The APIs make use of the Bearer Token that was provided through the steps executed in Authentication.  If the Bearer Token is not available, API calls will fail.

### Get Vehicles

NOTE: Use Get Vehicles first.

The Get Vehicles call retrieves a list of vehicles on your Tesla account.  It extracts the id_s variable for the first vehicle and stores it in the teslaVehicleIdS environment variable.  Without this value, other API calls will fail.  Once id_s has been retrieved, Get Vehicles should not be needed again.

### Wake Vehicle

The Wake Vehicle call wakes a sleeping vehicle.  A vehicle must be online to respond to requests for vehicle information.

The Tests contains a check named Vehicle On-Line Check.  If the test passes, the vehicle is online.

### Get Vehicle Data

The Get Vehicle Data call retrieves all available data for the vehicle defined by the teslaVehicleIdS environment variable.

