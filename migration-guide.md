## Migration Guide

<div class="alert alert-info-blue">
<p><strong>FAQ:</strong>
  <ul>
    <li><a href="/faq.html#how-to-update-aah-framework-to-the-latest-version">How to update aah framework to the latest version?</a></li>
    <li><a href="/faq.html#how-to-adapt-to-latest-aah-configuration">How to adapt to latest aah configuration?</a></li>
  </ul>
</p>
</div>


  * In `v0.10` file `init.go` introduced to evolve aah framework, please add it your version control.
  * As part Validator implementation, error handling evolved. So `aah.ErrorHandler` have been refactored into `aah.ErrorHandlerFunc`, [more info]({{aah_github_issues_url}}/132).
