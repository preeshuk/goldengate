<!--
    {
        "name":"Create the target connection and unlock the GGADMIN user",
        "description":"Create the target connection and unlock the GGADMIN user"
    }
-->
Follow the steps below to connect the target Autonomous Data Warehouse \(ADW\) instance.

1.  Use the Oracle Cloud Console navigation menu to navigate back to **GoldenGate**.

2.  Click **Connections** and then **Create Connection**.

    ![Create Connection in GoldenGate menu](https://oracle-livelabs.github.io/goldengate/ggs-common/create/images/04-02-connections.png " ")

3.  The Create connection panel appears. For Name, enter **TargetADW** and optionally, a description.

4.  From the **Compartment** dropdown, select a compartment.

5.  From the a Type dropdown, select **Oracle Autonomous Database**.

6.  For Database details, select **Select database**.

    ![Source Database details](https://oracle-livelabs.github.io/goldengate/ggs-common/create/images/04-06-create-connec-general-info.png " ")

7. Select a **Compartment** from the dropdown, and then select **TargetADW-&lt;numbers&gt;** from the dropdown. 

8. For Database username, enter `ggadmin`.

9. Select a **Compartment** from the dropdown, and then select a Database user password secret from the dropdown.

    > **NOTE:** This password will be used to unlock `GGADMIN` in a later task.

    ![Target Database details](https://oracle-livelabs.github.io/goldengate/ggs-common/create/images/04-09-create-connec-details.png)

10. Click **Advanced options**, and then click **Network connectivity**. Under Traffice routing method, select **Shared endpoint**.

11. Click **Create**.

    ![Target Database details](https://oracle-livelabs.github.io/goldengate/ggs-common/create/images/02-11-network-connect.png)

    The source and target databases appear in the list of Connections. The connection becomes Active after a few minutes.


