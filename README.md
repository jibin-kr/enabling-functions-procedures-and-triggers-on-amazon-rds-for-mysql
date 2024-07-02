# Enabling Functions, Procedures, and Triggers on Amazon RDS for MySQL DB Instances
============================================================

Amazon RDS is a managed service and doesn't provide system access (SUPER privileges). To enable functions, procedures, and triggers, you need to turn on binary logging and set `log_bin_trust_function_creators` to `true` in the custom database (DB) parameter group for your DB instance.

## Creating a DB Parameter Group

If you create a DB instance and don't specify a DB parameter group, Amazon RDS creates a new default DB parameter group. For more information, see [Working with parameter groups](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithParamGroups.html).

## Enabling Functions, Procedures, and Triggers

To turn on functions, procedures, and triggers for Amazon RDS for MySQL DB instances, complete the following steps:

### Step 1: Create a DB Parameter Group

Create a DB parameter group.

### Step 2: Modify the Custom DB Parameter Group

Modify the custom DB parameter group, and then set the parameter: log_bin_trust_function_creators=1
Choose **Save Changes**.

**Note:** Before you use the DB parameter group with a DB instance, wait at least 5 minutes.

### Step 3: Associate the Parameter Group with the DB Instance

In the navigation pane, choose **Databases**.

Choose the DB instance that you want to associate with the DB parameter group.

Choose **Modify**.

Select the parameter group that you want to associate with the DB instance.

Reboot the DB instance.

**Note:** The parameter group name changes immediately, but parameter group changes aren't applied until you reboot the instance without failover.

### If You Already Use a Custom Parameter Group

If you already use a custom parameter group, then complete only steps 2-3. The parameter `log_bin_trust_function_creators` is a dynamic parameter that doesn't require a DB reboot.

## Troubleshooting

When you turn on automated backup for a MySQL DB instance, you also turn on binary logging. When you create a trigger, you might receive the following error message:
ERROR 1419 (HY000): You don't have the SUPER privilege and binary logging is enabled (you might want to use the less safe log_bin_trust_function_creators variable)
If you receive this error, then modify the `log_bin_trust_function_creators` parameter to `1`. This allows functions, procedures, and triggers on your DB instance. If you still get access denied errors after you set the parameter to `1`, then see [How can I resolve 1227 and definer errors when importing data to my Amazon RDS for MySQL DB instance using mysqldump?](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/MySQL.Importing.html)

**Note:** When you set `log_bin_trust_function_creators=1`, unsafe events might be written to the binary log. Binary logging is statement-based (`binlog_format=STATEMENT`).

For more details about the parameter `log_bin_trust_function_creators`, see [log_bin_trust_function_creators and Stored program binary logging](https://dev.mysql.com/doc/refman/8.0/en/stored-programs-logging.html) on the MySQL website.
