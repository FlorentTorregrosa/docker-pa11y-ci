# Pa11y CI Command Line tool in a Docker container

This repo contains the build for the maintained docker container
`florenttorregrosa/pa11y-ci`.

## Use

### Sample docker-compose entry

A sample docker-compose file for this container is as follows:

```
version: '3.4'
services:
  pa11y:
    image: florenttorregrosa/pa11y-ci:0.10
    volumes:
       - ./config.json:/workspace/config.json
```

### Sample config.json

The pa11y-ci command line tool can take a config.json file that contains basic
configuration information, and a list of URLs to test.  This repo contains a
sample config.json that execute tests against the following URLs:

* http://pa11y.org
* http://pa11y.org/contributing
* https://www.google.com

A config.json example file is as follows (note as of this writing www.google.com
fails testing, while the other two URLs pass) that uses Google Chrome for browser
testing, and tests the above list of URLs against WCAG 2AA standards:

```json
{
  "defaults": {
    "standard": "WCAG2AA",
    "chromeLaunchConfig": {
      "args": [
        "--no-sandbox"
      ],
      "ignoreHTTPSErrors": false
    }
  },
  "urls": [
    "http://pa11y.org/",
    "http://pa11y.org/contributing",
    "https://www.google.com"
  ]
}
```

### Command Line execution using Docker Compose

To use this command line tool, execute the following from the command line with
docker-compose (assumes this container is set up as a service named "pa11y"):

```sh
docker-compose run pa11y_ci /bin/sh -c "pa11y-ci --config /workspace/config.json"
docker-compose run pa11y_ci /bin/sh -c "pa11y-ci --sitemap https://example.com/sitemap.xml --config /workspace/config.json"
docker-compose run --user=pptruser pa11y_ci /bin/sh -c "/node_modules/.bin/pa11y-ci --sitemap https://florent-torregrosa.fr/sitemap.xml --config /workspace/config.json"
```

## Additional Configuration

The Pa11y-ci command line tool has several configuration options, some of which
are discussed here. For more information, visit the [Pa11y Tutorials](http://pa11y.org/tutorials/).

### Setting a threshold before failing

The number of errors, warnings, or notices to permit before the test is
considered to have failed (with exit code 2) when running via the CLI.

```json
{
  "defaults": {
    "standard": "WCAG2AA",
    "threshold": 2
  }
}
```

### Ignoring warnings or notices
You can ignore warnings or notices using the below code. You can use
**includeNotices: true** and **includeWarnings: true** for on a page by page
basis.

```javascript
{
  "defaults": {
    "standard": "WCAG2AA",
    "ignore": [ "notice", "warning" ]
  },
  "urls": [
    {
      "url": "http://pa11y.org/",
      "includeWarnings": true
    }
  ]
}
```

### Setting a timeout
You can set a length of time before you'll receive a timeout error for the test.

```json
{
  "defaults": {
    "standard": "WCAG2AA",
    "timeout": 500
  }
}
```

### Excluding elements from testing

In some cases, we may not want to scan certain elements, such as third party
plugins we cant control. For example, below I'll skip the #ad-server element on
the home page.

```json
{
  "urls": [
    {
      "url": "http://pa11y.org/",
      "hideElements": "#ad-server"
    }
  ]
}
```

### Save a screenshot

You can set a file path to save a screen capture of the tested page to. The
screen will be captured immediately after the Pa11y tests have run so that you
can verify that the expected page was tested.

```javascript
{
  "urls": [
    {
      "url": "http://pa11y.org/",
      "screenCapture": "/path/to/folder/my-screen-capture.png"
    }
  ]
}
```

### Actions

Actions are additional interactions that you can make Pa11y perform before the
tests are run. They allow you to do things like click on a button, enter a value
in a form, wait for a redirect, or wait for the URL fragment to change:

```javascript
{
  "urls": [
    {
      "url": "http://pa11y.org/",
      "actions": [
        'click element #tab-1',
        'wait for element #tab-1-content to be visible',
        'set field #fullname to John Doe',
        'check field #terms-and-conditions',
        'uncheck field #subscribe-to-marketing',
        'screen capture example.png',
        'wait for fragment to be #page-2',
        'wait for path to not be /login',
        'wait for url to be https://example.com/',
        'wait for #my-image to emit load',
        'navigate to https://another-example.com/'
      ]
    }
  ]
}
```

## Resources
* [Project Website](http://pa11y.org/)
* [Project Github](https://github.com/pa11y)
* [Using Actions in Pa11y](http://hollsk.co.uk/posts/view/using-actions-in-pa11y)
* [Forked from Promet](https://github.com/promet/docker-pa11y-ci)
