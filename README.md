# fcp-fdm-performance-test-suite

A JMeter based test runner for the Farming Data Model (FDM).

- [Licence](#licence)
  - [About the licence](#about-the-licence)

## Build

Test suites are built automatically by the [.github/workflows/publish.yml](.github/workflows/publish.yml) action whenever a change are committed to the `main` branch.
A successful build results in a Docker container that is capable of running your tests on the CDP Platform and publishing the results to the CDP Portal.

## Run

The performance test suites are designed to be run from the CDP Portal.
The CDP Platform runs test suites in much the same way it runs any other service, it takes a docker image and runs it as an ECS task, automatically provisioning infrastructure as required.

## Local Testing with LocalStack

The `fcp-fdm-performance-test-suite` can be ran locally via Docker compose.

You will first need to run the service you wish to test against locally.

Once this is complete, you can trigger a local test run:

```bash
./test.sh
```

Test reports are saved locally and can be viewed in `reports` directory.

## Authentication

The test suite supports optional Microsoft Entra (Azure AD) authentication using OAuth 2.0 client credentials flow. This allows testing of authenticated APIs without manual token management.

### How It Works

1. **Token Acquisition**: During the Setup Thread Group, a single access token is requested from Microsoft Entra (`login.microsoftonline.com`)
2. **Token Sharing**: The token is extracted and stored in the `${access_token}` variable, shared across all test threads
3. **Authorization Header**: When enabled, each request to the Messages API automatically includes `Authorization: Bearer ${access_token}`

### Configuration

Authentication is controlled via environment variables in the `.env` file:

```env
# Enable/disable authentication
AUTH_ENABLED=true

# Microsoft Entra credentials
TENANT_ID=your-azure-tenant-id
CLIENT_ID=your-app-client-id
CLIENT_SECRET=your-app-secret
```

### Token Lifecycle

- Tokens are acquired **once** during test setup
- The token is shared across all threads
- The token expires after 60 minutes, they remain valid for the complete test run

### Disabling Authentication

To test unauthenticated endpoints or run tests in environments without authentication:

```env
AUTH_ENABLED=false
```

When disabled, the test suite skips token acquisition and does not add Authorization headers to requests.

## Licence

THIS INFORMATION IS LICENSED UNDER THE CONDITIONS OF THE OPEN GOVERNMENT LICENCE found at:

<http://www.nationalarchives.gov.uk/doc/open-government-licence/version/3>

The following attribution statement MUST be cited in your products and applications when using this information.

> Contains public sector information licensed under the Open Government licence v3

### About the licence

The Open Government Licence (OGL) was developed by the Controller of Her Majesty's Stationery Office (HMSO) to enable
information providers in the public sector to license the use and re-use of their information under a common open
licence.

It is designed to encourage use and re-use of information freely and flexibly, with only a few conditions.
