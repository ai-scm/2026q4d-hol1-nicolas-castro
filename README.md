# 2026q4d-hol1-nicolas-castro
This repository has the steps made in hol 1.

# CSV to JSON Pipeline on AWS (S3 + Lambda + Glue + Athena)

## Steps

### 1. Create folders in the existing S3 buckets
Two predefined buckets were used, one for input and one for output. A folder named `nicolas-castro` was created in each:
- **Input:** `s3://bld-tseed-workshop-input-semillero-2026-q4/nicolas-castro/`
- **Output:** `s3://bld-tseed-workshop-output-semillero-2026-q4-d/nicolas-castro/`

### 2. Create a Lambda function
A Python Lambda function (`bld-tseed-workshop-nicolas-castro`) reads the CSV file from the input folder, converts each row into a JSON file and saves it in the output bucket's folder.
- Configured through environment variables: `SOURCE_BUCKET`, `SOURCE_PREFIX`, `DEST_BUCKET`, `DEST_PREFIX`.
- The Lambda role was given `s3:ListBucket` and `s3:GetObject` on the input bucket and `s3:PutObject` on the output bucket.
- Each JSON record is written on a single line so Athena can read it.

### 3. Use AWS Glue (Crawler)
A Glue Crawler scans the JSON files in the output bucket, infers their schema and creates a table (`nicolas_castro`) in the `bld-tseed-workshop-nicolas-castro` database, turning the files into structured data.

### 4. Query the data with Athena
After setting an S3 location for query results, the table can be queried with SQL:

```sql
SELECT * FROM "bld-tseed-workshop-nicolas-castro"."nicolas_castro" LIMIT 10;
```