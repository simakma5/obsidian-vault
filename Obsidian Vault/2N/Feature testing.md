# Feature exploration

## Research the epic first

For every notable feature, we must clearly identify which parts of the system might be impacted by its development. This helps determine what needs to be tested with special care.

### Standard workflow

Explore and test the standard workflow as intended by the development team.

### Wrong scenario

Come up with a scenario where an unsuspecting customer uses the feature wrong.

### Licenses

- Is the feature restricted to a specific license tier?
- What happens when I enable it in trial mode and then activate basic?
- Is the license check performed properly via API, or is it just a lazy FE check?

### User roles

- Is the feature restricted to specific user roles?
- What if the user is an admin and then is demoted to an unsupported role?

### API

Explore the possibility of handling the feature through API along with automation options.

### Database

- Does the feature involve changes in databases?
- Is it PostgreSQL or InfluxDB?

## General approach

First of all, we need to verify that all user stories and bugs regarding the feature-specifying epic are done. If there is an unresolved issue, it doesn't necessarily mean the feature is not done / cannot be tested. In this case, we simply have to discuss with the development team and evaluate the importance of having the corresponding issue resolved before feature testing.

> [!Warning] According to user stories and beyond!
> As testers, we should avoid the careless method of merely checking off acceptance criteria one by one. Instead, always consider the practical use cases, test beyond the feature specifications, and do your best to challenge the system!
>
> For example, the feature functionality should always be tested both on clean and upgraded installations!

The tester responsible for feature tests must have a comprehensive understanding of the feature's functionality and specifications. This makes them naturally accountable for adding new use cases to the regression test suite to cover the new feature. This is usually done after the bug-fix development phase. However, it is advisable to keep track of the testing procedure from the beginning. For the same reasons, testers should already consider various automation options and evaluate their sufficiency compared to manual testing during regression testing.
