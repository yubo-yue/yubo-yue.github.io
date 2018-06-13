---
layout: post
title:  "Thinking on 'Design Data-intensive application' (1) "
---

sudo -u ssof /apollo/bin/env -e FBASellerCapsDataTurbo /apollo/env/FBASellerCapsDataTurbo/bin/launch-cluster-run-app.sh \
--springConf=spring-configuration/SMSIPIMetrics.xml \
--instanceCount=4 --instanceType=m4.xlarge \
--myIPIScripts=hive-scripts/sms/inventory_performance_metrics/ExportIPIMetrics.q \
--myUpdateLatestDateScripts=hive-scripts/sms/UpdateLastUpdateDate.q \
--ddb_endpoint=dynamodb.us-east-1.amazonaws.com \
--marketplace_ids=1 \
--ddb_throughput_percent=1.0 \
--run_date=2018-05-30 \
--region_id=1 \
--sms_s3_bucket=sms-us-east-1 \
--clusterName=yubo-test-2018-05-30


brazil ws env attach --alias dev-dsk-yubo-2b-8ff10926.us-west-2.amazon.com/FBASellerCapsDataTurbo
brazil ws env update --alias dev-dsk-yubo-2b-8ff10926.us-west-2.amazon.com/FBASellerCapsDataTurbo



sudo -u ssof /apollo/bin/env -e FBASellerCapsDataTurbo /apollo/env/FBASellerCapsDataTurbo/bin/launch-cluster-run-app.sh \
--springConf=spring-configuration/ExportCurrentLimit.xml \
--instanceCount=4 \
--instanceType=m4.xlarge \
--ddb_endpoint=dynamodb.us-east-1.amazonaws.com \
--marketplace_ids=1 \
--ddb_throughput_percent=1.0 \
--run_date=2018-06-12 \
--schedule_date=2018-06-12 \
--region_str=us \
--sms_s3_bucket=sms-us-east-1 \
--s3_bucket=sms-us-east-1-devo \
--clusterName=yubo-test-2018-06-12 \
--interactive

sudo -u ssof /apollo/bin/env -e FBASellerCapsDataTurbo /apollo/env/FBASellerCapsDataTurbo/bin/launch-cluster-run-app.sh \
--springConf=spring-configuration/ExportCurrentLimit.xml \
--instanceCount=4 \
--instanceType=m4.xlarge \
--ddb_endpoint=dynamodb.us-east-1.amazonaws.com \
--marketplace_ids=1 \
--ddb_throughput_percent=1.0 \
--run_date=2018-06-12 \
--schedule_date=2018-06-12 \
--region_str=us \
--sms_s3_bucket=sms-us-east-1 \
--s3_bucket=sms-us-east-1-devo \
--clusterName=yubo-test-2018-06-12-1 

sudo -u ssof /apollo/bin/env -e FBASellerCapsDataTurbo /apollo/env/FBASellerCapsDataTurbo/bin/submit-turbo-job.sh \
--springConf=spring-configuration/ExportCurrentLimit.xml \
--instanceCount=4 \
--instanceType=m4.xlarge \
--ddb_endpoint=dynamodb.us-east-1.amazonaws.com \
--marketplace_ids=1 \
--ddb_throughput_percent=1.0 \
--run_date=2018-06-12 \
--schedule_date=2018-06-12 \
--region_str=us \
--sms_s3_bucket=sms-us-east-1 \
--s3_bucket=sms-us-east-1-devo \
--clusterName=yubo-test-2018-06-12 


DROP TABLE IF EXISTS SMS_POLICY_SOURCE_TAG_OUTPUT;
CREATE EXTERNAL TABLE SMS_POLICY_SOURCE_TAG_OUTPUT (
    POLICY_SOURCE STRING,
    POLICY_SOURCE_TAG STRING
)
PARTITIONED BY (REGION_STR STRING, SNAPSHOT_DATE STRING)
ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
STORED AS TEXTFILE
LOCATION 's3://sms-us-east-1-devo/current_limits/policy_source_tag';

--ALTER TABLE SMS_POLICY_SOURCE_TAG_OUTPUT ADD PARTITION(REGION_STR='us', SNAPSHOT_DATE='2018-06-12');

SET hive.exec.stagingdir=/tmp/hive/;
SET hive.exec.scratchdir=/tmp/hive/;

INSERT OVERWRITE TABLE SMS_POLICY_SOURCE_TAG_OUTPUT
PARTITION(REGION_STR='us', SNAPSHOT_DATE='2018-06-12')
SELECT
    *
FROM
    SMS_POLICY_SOURCE_TAG
;

Screen
ctrl+c+N/P (preview and next)


