---
import:
  - InstallWizard/Partial/Crontab
---
# [Install Wizard](/InstallWizard) > [Linux/BSD](/InstallWizard/Linux) > Scheduling

## Configuration
Before scheduling FlexGet you must must [write a configuration file](/Configuration) and test that it works correctly.  The SQLite database file will get created in the same directory with the configuration file, so please make sure the user executing flexget has write access to that path.


## Scheduling
{{> InstallWizard/Partial/Crontab }}


## More options

Further more complicated upstart jobs etc can be found [Linux/AutoStart](/InstallWizard/Linux/AutoStart)

# Completed!
Sit back and relax.