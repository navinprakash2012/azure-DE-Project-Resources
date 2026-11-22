Create source table to ingest data from git repo -
CREATE TABLE source_cars_data (
    Branch_ID   VARCHAR(200),
    Dealer_ID   VARCHAR(200),
    Model_ID    VARCHAR(200),
    Revenue     BIGINT,
    Units_Sold  BIGINT,
    Date_ID     VARCHAR(200),
    Day         INT,
    Month       INT,
    Year        INT,
    BranchName  VARCHAR(2000),
    DealerName	VARCHAR(2000),
    Product_Name    VARCHAR(200)
);

Create stored procedure and the same will used in updating the dates for incremental process -
CREATE PROCEDURE UpdateWatermarkTable
    @lastload VARCHAR(200)
AS
BEGIN
    --start the transaction
    BEGIN TRANSACTION;
    --Update the incremental column in the table
    Update water_table
    SET last_load = @lastload
    COMMIT TRANSACTION;
    END;

The last load date will be uploaded to the water_table -
create table water_table (
    last_load VARCHAR(2000)
)
