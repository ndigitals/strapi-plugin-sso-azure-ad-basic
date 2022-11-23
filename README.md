# Strapi plugin sso-azure-ad-basic

[![NPM version](https://badge.fury.io/js/%40ndigitals%2Fstrapi-plugin-sso-azure-ad-basic.svg)](https://badge.fury.io/js/%40ndigitals%2Fstrapi-plugin-sso-azure-ad-basic)
[![Build Status](https://github.com/ndigitals/strapi-plugin-sso-azure-ad-basic/actions/workflows/release.yml/badge.svg)](https://github.com/ndigitals/strapi-plugin-sso-azure-ad-basic/actions/workflows/release.yml)
[![NPM Downloads](https://img.shields.io/npm/dm/@ndigitals/strapi-plugin-sso-azure-ad-basic)](https://www.npmjs.com/package/@ndigitals/strapi-plugin-sso-azure-ad-basic)
[![MIT License](https://img.shields.io/apm/l/atomic-design-ui.svg?)](https://github.com/tterb/atomic-design-ui/blob/master/LICENSEs)

> This is an updated fork of the [original Strapi plugin](https://www.npmjs.com/package/strapi-plugin-sso-azure-ad-basic).

This is a basic plugin using Azure Active Directory identity for Single Sign On (SSO).
The configurations are controlled by environment variables inside `.env` file.

## How it works

Normally you get the token to authenticate and authorize your app to access Azure services. Can I use it to authorize Strapi services? Yes by using azure token to authenticate a user in Strapi. The idea is to map Azure AD roles with the Strapi roles. If the role match then the user will be authenticated and it will return Strapi generated JWT token as if the user logs in. This works even if the user is not yet created. If authenticated and the role match the user will be created automatically. If the user already exist, the user information will be updated. This way you can use it to authorize Azure services at the same time authorize your own Strapi services by either using the Strapi token or the azure token.

To authenticate, use the returned token as parameter. After authenticated, it will still use the Strapi jwt token since you are just accessing its internal APIs.

This will work both for admin and API users.

For API users, in the client pass the azureToken (token returned after logging in from Azure) to /verifyTokenAPIUser endpoint.

## Supported Strapi versions

Strapi v3.6.x and above

## Installation

```shell
npm install @ndigitals/strapi-plugin-sso-azure-ad-basic --save
```

or

```shell
yarn add @ndigitals/strapi-plugin-sso-azure-ad-basic
```

## Configuration/Setup

Inside plugin strapi-files copy `admin` to `admin/` project root directory and copy `hooks.js` to Strapi `config`. Also copy the `hooks` folder to the Strapi root directory. If the folders already exist, only copy the files or code that are missing.

### Setup up environment variables

Create .env if not yet available on the project root directory

Add the following variables:

```
AZURE_AD_ROLE_MAPPING=[{"azureRole":"Application Administrator","strapiRole":"Super Admin"},{"azureRole":"Application Developer","strapiRole":"Technologist"}]
AZURE_AD_ROLE_MAPPING_API_USERS=[{"azureRole":"Application Administrator","strapiRole":"Authenticated"}]
AZURE_AD_CLIENT_ID=57e34ea2-2dae-4445-b7dc-5320cdfc969c
AZURE_AD_REDIRECT_URL=http://localhost:8000/admin/auth/login
```

~~Note that `AZURE_AD_ROLE_MAPPING` and `AZURE_AD_ROLE_MAPPING_API_USERS` is a string representation of an array of objects~~

~~It is required that you map your Strapi roles to the corresponding role from Azure AD.~~

### Copy hooks file and folder

This will make sure that the verifytoken and verifytokenapiuser api is public

## Build and run your project

```shell
npm run build && npm run develop
```

or

```shell
yarn build && yarn develop
```
