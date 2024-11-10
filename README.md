# Sparkify Data Warehouse & ETL Pipeline

## Project Overview

This project involves building a data warehouse on AWS Redshift for Sparkify, a music streaming startup. Sparkify wants to analyze user activity and song play patterns at scale, which requires moving data from its current S3 storage to a robust Redshift-based warehouse. An ETL (Extract, Transform, Load) pipeline is implemented to support data extraction, transformation, and loading to facilitate analysis by the Sparkify analytics team.

## Purpose

The ETL pipeline and data warehouse aim to:
- **Analyze User Listening Behavior**: Identify popular songs, understand user engagement patterns, and track peak usage times.
- **Enable Data-Driven Decisions**: Sparkify’s analytics team can make informed choices about content and user experience.
- **Support Scalability**: As Sparkify grows, this solution provides a scalable foundation for handling increasing data loads.

## System Architecture

1. **AWS S3**: Stores raw JSON data files in two directories:
   - **Log Data**: JSON files detailing user activities within the app.
   - **Song Data**: JSON files containing metadata on the songs available in the app.

2. **AWS Redshift**: Acts as the data warehouse, staging and transforming data into a star schema format optimized for analytics.

3. **ETL Pipeline**: A Python-based pipeline that:
   - **Extracts** data from S3,
   - **Stages** data in Redshift,
   - **Transforms** data into fact and dimension tables.

4. **Security**: IAM roles and policies provide Redshift with read-only access to S3 and secure the data flow.

## Database Schema Design

The star schema model includes one **Fact Table** and four **Dimension Tables**:

### Fact Table
- **songplays**: Records each song play event with columns like `songplay_id`, `start_time`, `user_id`, `song_id`, `artist_id`, `session_id`, `location`, and `user_agent`.

### Dimension Tables
1. **users**: Contains user information (`user_id`, `first_name`, `last_name`, `gender`, `level`).
2. **songs**: Stores song metadata (`song_id`, `title`, `artist_id`, `year`, `duration`).
3. **artists**: Contains artist details (`artist_id`, `name`, `location`, `latitude`, `longitude`).
4. **time**: Provides time breakdowns (`start_time`, `hour`, `day`, `week`, `month`, `year`, `weekday`).

This star schema structure supports quick analytical queries and optimized performance for large datasets.

## ETL Pipeline

### Steps:
1. **Extract**:
   - Connect to the S3 bucket where JSON logs and metadata are stored.
   - Read the log and song JSON files from the designated S3 directories.

2. **Stage Data in Redshift**:
   - Load data into Redshift staging tables (`staging_events` for logs and `staging_songs` for song metadata).
   - This ensures the data is accessible for further transformation.

3. **Transform and Load to Analytics Tables**:
   - Use SQL queries to insert data into correct tables.
   - Run `python create_tables.py` followed by `python etl.py` to load tables.