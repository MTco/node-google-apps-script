---
### :warning: A request for maintainers :construction:

This project was initially developed for my own personal use at work and reached a wider audience with the help of a few members on the Google Apps Script team. I do not have the capacity to maintain this project and I am asking you, the beloved community member, for help.

While the core commands are mostly stable, users have identified a few bugs and asked for some additional features. I need help taking action on these [issues](https://github.com/danthareja/node-google-apps-script/issues) and [pull requests](https://github.com/danthareja/node-google-apps-script/pulls), ideally from someone who uses this project in production.

If you are interested in taking over this project, please [contact me](mailto:danthareja@gmail.com). I'm happy to provide anything you need in order to continue improving this useful tool for the community.

Until a candidate has been selected for take over, I will not be able to review issues and pull requests. I encourage anyone interested in active development to do so in a fork.

---

# gapps (Google Apps Script)
>The easiest way to develop Google Apps Script projects

Using **gapps**, you can develop your Apps Script locally and push files to the Apps Script servers. This allows you to use any editor of your choice, version control, and other modern webdev patterns in to Apps Script development.

## Requirements
  - [node v0.12.x](https://nodejs.org/download/)
  - `npm install -g node-google-apps-script`

## Quickstart

### 1. Get Google Drive Credentials

You can do this one of two ways:

1. Add Drive permissions to the default Developer Console Project that is created for each Apps Script project
1. Use an independent Developer Console Project and enable the Drive API

#### 1.1 Default Apps Script Developer Console Project

1. Go to the [Google Cloud Platform](https://console.cloud.google.com)
1. Access the automatically created Google Apps Script Project `API Project`: click on the blue button at the top and select (`API Project - api-project-##########`) from the list.
1. Enable the Google Drive API
   1. Click `APIs & services` in the left nav and then select `Library`
   1. Search for `Drive` and select the Google Drive API listing.
   1. Click `Enable API`
1. Acquire Google Drive Client Secret Credentials
   1. If you just enabled the Drive API, hit `Create Credentials`, else, go to `Credentials` in the left menu, select `Create credentials` and choose `OAuth client ID`
   1. Type in any `Product name` in the OAuth consent screen
   1. In the menu that appears, choose `Other` for the `Application type`.
   1. Give it any name you like and click `Create`.
   1. Finally, download your credentials using the `Download as JSON` button to the right. Save these credentials to a location of your choosing; `~/Downloads` is fine.
   1. You may close the Developer Console window.

#### 1.2 Independent Developer Console Project

1. Create a new Dev Console Project
   1. Use [this link](https://console.developers.google.com/start/api?id=drive&credential=client_key) to create the project. It will auto-activate the Google Drive API. If you have multiple Google Accounts, append `&authuser=1` to the end of the url to choose which account to login with. Note that `authuser` is zero-indexed.
   1. Make sure `Create a New Project` is selected and hit `Continue`.
   1. Once the project has been created, click `Go to Credentials`.
1. Acquire Google Drive Client Secret Credentials
   1. Select `Add credentials`, choose the `OAuth 2.0 client ID` type
   1. Click `Configure Consent Screen`.
   1. Select your email address from the dropdown and assign your add-on a Project Name. This can always be changed later.
   1. `Save` your Consent Screen
   1. In the menu that appears, choose `Other` for the `Application type`.
   1. Give it any name you like and click `Create`.
   1. Finally, download your credentials using the `Download as JSON` button to the right. Save these credentials to a location of your choosing; `~/Downloads` is fine.
   1. You may close the Developer Console window.

To return to this project later, select `Resources` > `Developer Console Project` while editing your script. Then click the link at the top of the dialog to open the Developer Console with this project selected.
    
### 2. Authenticate `gapps`

This process will set up Google Drive authentication to allow uploading and importing of the Apps Script project.

1. Run `gapps auth path/to/client_secret_abcd.json` i.e. `gapps auth ~/Downloads/client_secret_1234567890-abcd.apps.googleusercontent.com.json`
1. Follow the directions by clicking on the link generated by the script.
1. After you're successfully authenticated, feel free to delete the `client_secret.json` credentials file.

You can pass the option `--no-launch-browser` to generate a url that will give you a code to paste back into the console. This is useful if you're using ssh to develop.

### 3. Initialize your project

This proces will create `gapps.config.json` and a sub-directory where all Apps Script project files will be downloaded to. You can either use an existing project or create a new one.

#### 3.1 An existing Apps Script project

1. Navigate to your Apps Script project from Google Drive (must be a [standalone script](https://developers.google.com/apps-script/guides/standalone))
1. Get your project ID from the address bar, located after `/d/` and before `/edit` i.e. '//script.google.com/a/google.com/d/__abc123-xyz098__/edit?usp=drive_web'
1. Navigate to a directory where your Apps Script project will live
1. Run `gapps init <fileId>` within your project directory i.e. `gapps init abc123-xyz098`

#### 3.2 A new Apps Script project

1. Head to [https://script.google.com](https://script.google.com) and create a new blank project.
1. Save the mostly empty project and give it a name. Saving is important; the project is not in your Google Drive until it is saved.
1. Get your project ID from the address bar, located after `/d/` and before `/edit` i.e. '//script.google.com/a/google.com/d/__abc123-xyz098__/edit?usp=drive_web'
1. Navigate to a directory where your Apps Script project will live
1. Run `gapps init <fileId>` within your project directory i.e. example, `gapps init abc123-xyz098`


### 4. Develop locally

Start scripting and enjoy total freedom of your local dev environment and source control options! Create script files with a `.gs` or `.js` extension and html files with a `.html` extension

### 5. Upload New Files

Run `gapps upload` from within your project directory. You should then be able to reload your Apps Script project in the browser and see the changes.


## Best Practices

Check out Matt Hessinger's blog post: [Advanced development process with apps](http://googledevelopers.blogspot.com/2015/12/advanced-development-process-with-apps.html)

## Docs

### gapps auth

```
  Usage: gapps auth [options] <path/to/client/secret.json>

  Authorize gapps to use the Google Drive API

  Options:

    -b, --no-launch-browser  Do not use a local webserver to capture oauth code
                              and instead require copy/paste of key returned in
                              the browser after authorization completes.
    -p, --port [port]        Port to use for webserver
```

Performs the authentication flow described in the quickstart above.

### gapps init

```
  Usage: gapps init|clone <fileId> [options]

  Initialize project locally. The external Apps Script project must exist.

  Options:
    -k, --key [key]
    -s, --subdir [subdir]
    -o, --overwrite
```

Creates `gapps.config.json`, which contains information about your Apps Script project and downloads all project files into a subdirectory (default: `src/`)

### gapps upload

```
  Usage: gapps upload|push

  Upload back to Google Drive. Run from root of project directory
```

Upload the project to Google Drive. Sources files from `./src` or the
configured subdirectory.

### gapps oauth-callback-url

```
  Usage: gapps deployment oauth-callback-url

  Get the OAuth Callback URL for a project
```

Returns the OAuth Callback URL required by most 3rd-party OAuth services.

## Development
Please submit any bugs to the Issues page. Pull Requests also welcome.

If you want to develop, clone down the repo and have at it! You can run `npm link` from the root directory of the repo to symlink to your local copy. You'll have to uninstall the production version first `npm uninstall -g node-google-apps-script`.

## Limitations

`gapps` allows you to nest files in folders, but the Apps Script platform expects a flat file structure. Because of this, **no files can have the same name, even if they are in separate directories**. One file will overwrite the other, making debugging difficult.

Your add-on must be developed as a [standalone script](https://developers.google.com/apps-script/guides/standalone)
and tested within Doc or Sheet. This means that it cannot use certain functions of [bound scripts](https://developers.google.com/apps-script/guides/bound), namely installable triggers. While testing within a Doc, you have access to the
"Special methods" mentioned in [the docs](https://developers.google.com/apps-script/guides/bound), though. If
you followed the quickstart above, you should be set up correctly.

## Common Issues

The Google Drive API frequently returns a *400* error without a helpful error message. Common causes for this are:

- **javascript formatting errors**
  - The server-side javascript code (`.js` or `.gs`) is validated upon upload.
  - If there is a syntax error, the upload will fail.
- The project does not exist yet.
  - Verify that the project exists in your Google Drive folder.
  - If you have been added to an existing project, verify that the owner has shared the file with you with the "can edit" permission.
- Missing server-side libraries
  - verify that any required Libraries (frequently `Underscore` or `OAuth2`) have been added to the Apps Script project
- Authentication error
  - Verify that `~/.gapps` exists and has 4 values: `client_id`, `client_secret`, `redirect_uri`, and `refresh_token`
  - To reset authentication, 'rm ~/.gapps' and then perform the authentication steps in part #2 of the quickstart again.

## Acknowledgements

Huge thanks to [Shrugs](https://github.com/Shrugs) for a massive PR that bumped this project to v1.0

## Extra Resources

- [Google Apps Script Documentation](https://developers.google.com/apps-script/)
