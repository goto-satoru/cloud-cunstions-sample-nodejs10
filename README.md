# Google Cloud Functions - stop GCE instances sample

forked from https://github.com/GoogleCloudPlatform/nodejs-docs-samples.git

reference document: https://cloud.google.com/scheduler/docs/start-and-stop-compute-engine-instances-on-a-schedule

## create Pub/Sub topics

```
gcloud pubsub topics create stop-instance-event
gcloud pubsub topics list
```

## deploy function

Do NOT use Node.js 8 run time

```
git clone https://github.com/goto-satoru/cloud-functions-sample-nodejs10.git
cd cloud-functions-sample-nodejs10/stop-instance
gcloud functions deploy stopInstancePubSub --trigger-topic stop-instance-event --runtime nodejs10 --region=asia-northeast1
gcloud functions list
```

## create scheduler jobs

stop all GCE instances in asia-northeast1-c and asia-northeast1-b regions which have env=test label.

```
gcloud beta scheduler jobs create pubsub stop-instances --schedule '0 23 * * *' --topic stop-instance-event --message-body '{"zone":"asia-northeast1-a", "label":"env=test"}' --time-zone 'Asia/Tokyo'
gcloud beta scheduler jobs create pubsub stop-instances --schedule '0 23 * * *' --topic stop-instance-event --message-body '{"zone":"asia-northeast1-b", "label":"env=test"}' --time-zone 'Asia/Tokyo'
gcloud beta scheduler jobs create pubsub stop-instances --schedule '0 23 * * *' --topic stop-instance-event --message-body '{"zone":"asia-northeast1-c", "label":"env=test"}' --time-zone 'Asia/Tokyo'
gcloud beta scheduler jobs list
```
