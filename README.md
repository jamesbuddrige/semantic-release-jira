# semantic-release-jira-releases

[**semantic-release**](https://github.com/semantic-release/semantic-release) plugin to publish a jira release.

[![npm latest version](https://img.shields.io/npm/v/@jamesbuddrige/semantic-release-jira/latest.svg)](https://www.npmjs.com/package/@jamesbuddrige/semantic-release-jira)


| Step               | Description                                                                                                                                   |
|--------------------|----------------------------------------------------------------------------|
| `verifyConditions` | Validate the config options and check for a `JIRA_AUTH` in the environment |
| `sucess`           | Find all tickets from commits and add them to a new release on JIRA        |

## Install

```bash
$ npm install --save-dev @jamesbuddrige/semantic-release-jira
$ yarn add --dev @jamesbuddrige/semantic-release-jira
```

### Configuration
The plugin should be added to your config
```json
{
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    "@semantic-release/git",
    ["@jamesbuddrige/semantic-release-jira", {
      "projectId": "ABC",
      "releaseNameTemplate": "Test v${version}",
      "jiraHost": "https://jira-domain.atlassian.net",
      "ticketPrefixes": [ "TEST", "UH"],
      "ticketRegex": "[a-zA-Z]{3,5}-\\d{3,5}"
    }]
  ]
}
```
Please note that `ticketRegex` cannot be used together with `ticketPrefixes`.
```typescript
interface Config {
  /**
   * A domain of a jira instance ie: `uphabit.atlasian.net`
   */
  jiraHost: string;

  /**
   * A list of prefixes to match when looking for tickets in commits. Cannot be used together with ticketRegex.
   *
   * ie. ['TEST'] would match `TEST-123` and `TEST-456`
   */
  ticketPrefixes?: string[];

  /**
   * A unescaped regex to match tickets in commits (without slashes). Cannot be used together with ticketPrefixes.
   *
   * ie. [a-zA-Z]{4}-\d{3,5} would match any ticket with 3 letters a dash and 3 to 5 numbers, such as `TEST-456`, `TEST-5643` and `TEST-56432`
   */
  ticketRegex?: string;

  /**
   * The id or key for the project releases will be created in
   */
  projectId: string;

  /**
   * A lodash template with a single `version` variable
   * defaults to `v${version}` which results in a version that is named like `v1.0.0`
   * ex: `Semantic Release v${version}` results in `Semantic Release v1.0.0`
   *
   * @default `v${version}`
   */
  releaseNameTemplate?: string;

  /**
   * A lodash template for the release.description field
   *
   * template variables:
   *    version: the sem-ver version ex.: 1.2.3
   *      notes: The full release notes: This may be very large
   *             Only use it if you have very small releases
   *
   * @default `Automated released with semantic-release-jira-releases https://git.io/JvAbj`
   */
  releaseDescriptionTemplate?: string;

  /**
   * The number of maximum parallel network calls, default 10
   */
  networkConcurrency?: number;

  /**
   * Indicates if a new release created in jira should be set as released
   */
  released?: boolean;
  
  /**
   * Include the release date when creating a release in jira
   */
  setReleaseDate?: boolean;

  /**
   * Only create a release in the default channel (no pre-release channels)
   */
  onlyDefaultChannel?: boolean;
}
```
