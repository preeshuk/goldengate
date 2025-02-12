
# Create artifacts for the Stream Analytics pipeline

## Introduction

This lab walks you through steps to create supporting artifacts for your Stream Analytics pipeline to monitor the public transportation in a marked geographical location.

*Estimated Time*: 30 minutes

### Objectives
In this Lab you will:
- Download a json file which contains the sample data to be used in this tutorial
- Create a Kafka Connection
- Create a File Stream using the json file
- Create a Geo Fence
- Create a Database Connection
- Create a Database Reference

### Prerequisites

- You should have successfully completed all the previous labs.
- A json file with the sample data to be used in this tutorial:
  [Click here to download file.](./files/bus.json?download=1)


## **Task 1:** Create a Kafka Connection

1. On the **Catalog** page, click **Create New Item**, and select **Connection**.

2. Select **Kafka** from the submenu, to display the **Create Connection** screen.

3. On the **Type Properties** screen, enter the following details:
    -  In the **Name** field, enter **LocalKafka**.
    -  In the **Display Name**, enter a display name for the connection. If left blank, the **Name** field value is copied.
    -  In the **Description**, enter a meaningful description. **Connection to Kafka running locally** in this example.
    -  In the **Tags** field, enter **transportation, tutorial**.
    -  In the **Connection Type** drop-down, the selected connection is displayed. **Kafka** in this example.

    ![Creating a Kafka Connection](./images/kafkaconn1.png "")

4. On the **Connection Details** page, enter **localhost** in the **Zookeepers** field.

5. Click **Test Connection**, and wait till the status is successful. Click **Save**.

    ![Testing a Kafka Connection](./images/kafka-conn2.png "")


## **Task 2:** Create a File Stream

1. On the Catalog page, **Create New Item**, and select **Stream**.

2. Select **File** from the submenu, to display the **Create Stream** screen.

3. On the **Type Properties** screen, enter the following details:
    - In the **Name** field, enter **BusStream**.
    - In the **Display Name** field, enter a display name for the stream. If left blank, the **Name** field value is   copied.
    - In the **Tags** field, enter **transportation, tutorial**.
    - In the **Stream Type** drop-down, the selected stream is displayed. **File** in this example.

    ![Selecting Type Properties for the Stream file](./images/filestream.png "")

4. On the **Source Details** page:
    - Click **Upload** to upload the **busstream.json** file that you downloaded earlier.
    - Select **JSON** from the **Data Format** drop-down.

    ![Uploading an input stream file](./images/upldjsonfile.png "")

5. On the **Shape** page:
    -  Select the **Infer Shape** option and click **Infer**.
    -  Wait till the Shape is successfully inferred from the input stream.
    -  In the **Shape Name** field, enter **BusStream**.

    ![Inferring a Shape](./images/fstreamshape.png "")

6. Click **Save**.

## **Task 3:** Create a GeoFence

1. On the Catalog page, **Create New Item**, and select **Geo Fence**.

2. Select **Geo Fence** from the submenu, to display the **Create Geo Fence** screen.

3. On the **Type Properties** screen, enter the following details:
    - In the **Name** field, enter **AtlantaRegions**.
    - In the **Display Name** field, enter a display name for the geo fence. If left blank, the **Name** field value is copied.
    - In the **Description** field, enter **Geofence to include Atlanta Regions**.
    - In the **Tags** field, enter **transportation, tutorial**.
    - In the **Geo Fence Type** drop-down, select **Manually Created Geo Fence**.

![Selecting Type Properties for the Manual Geofence](./images/gf1.png "")

4. Click **Save**. A page with the world map is displayed. Mark a geo fence around Atlanta. Use the **Marquee Zoom** to zoom-in a specific area.

![Creating a Manual Geofence](./images/create-geofence.png "")

5. On top left of the page, click the **Polygon** tool on the toolbar and do the following:
    - Start marking the geofence on the map. When you are done, click the **Polygon** tool again.
    - In the **Name** field on the right side, enter **Atlanta**.
    - In the **Description** field, enter **Monitor public transport buses around Atlanta**.
    - On the left side, click the **Save** icon to save your changes.

6. Click **Return to Catalog**.

## **Task 4:** Create a Database Connection

1. On the Catalog page, **Create New Item**, and select **Generic Database**.

2. Select **Generic Database** from the submenu, to display the **Create Connection** screen.

3. On the **Type Properties** screen, enter the following details and click **Next**:
    - In the **Name** field, enter **LocalMySQLDB**.
    - In the **Display Name** field, enter a display name for the Connection. If left blank, the **Name** field value is copied.
    - In the **Tags** field, enter **tutorial, transportation**.

![Creating a Database Connection](./images/dbconn1.png "")


4. On the **Connection Details** page, enter the following details:
    - In the **database** field, select **MySQL** from the dropdown.
    - In the **jdbc url** field, enter **jdbc:mysql://OSA _ DEMO:Welcome123!@localhost:3306/OSA _ DEMO**.

![Creating a Database Connection](./images/dbconn2.png "")

5. Click **Test Connection**, and wait till the status is successful. Click **Save**.

## **Task 5:** Create a Database Reference

1. On the Catalog page, **Create New Item**, and select **Reference**.

2. Select **Database Table** from the submenu, to display the **Create Reference** screen.

3. On the **Type Properties** screen, enter the following details and click **Next**:
    - In the **Name** field, enter **DriverDetails**.
    - In the **Display Name** field, enter a display name for the Reference. If left blank, the **Name** field value is copied.
    - In the **Description**, enter **Database Reference to fetch Driver Details**.
    - In the **Tags** field, enter **tutorial, transportation**.

![Selecting Type Properties for the Database Reference](./images/refer1.png "")

4. On the **Source Details** page, select the **LocalMySQLDB** connection that you created earlier, and click **Next**.

![Selecting a Database Connection](./images/refer2.png "")

5. On the **Shape** page, select **driver_details** from the **Shape Name** drop-down. Click **Save**.

![Selecting a Database Table](./images/refer3.png "")

You may now **proceed to the next lab**.

## Learn More

* [Transform and Analyze Data Streams](https://docs.oracle.com/en/middleware/fusion-middleware/osa/19.1/using/creating-pipeline-transform-and-analyze-data-streams.html#GUID-9DB9B57A-1095-4557-ACB9-816A696EB121)

* [Installing GoldenGate Stream Analytics](https://docs.oracle.com/en/middleware/fusion-middleware/osa/19.1/using/install-and-upgrade.html#GUID-A687DFF0-728A-4071-947A-F3E90ABF65F6).

* [GoldenGate Stream Analytics on Oracle Cloud Marketplace](https://docs.oracle.com/en/middleware/fusion-middleware/osa/19.1/osamp/getting-started-goldengate-stream-analytics-oci.html#GUID-B488861E-1C43-4177-A1F8-40F8E44754AD).

* [Mandatory RTE Configurations](https://docs.oracle.com/en/middleware/fusion-middleware/osa/19.1/using/configuring-runtime-environment.html#GUID-EB33DDFD-7444-434D-8944-059564A453FD).

## Acknowledgements
* **Author** - Pratibha Manjunath, Senior User Assistance Developer, GoldenGate Stream Analytics
* **Contributors** - Sumathi Nelapati
* **Last Updated By/Date** - Pratibha Manjunath, October 2024
