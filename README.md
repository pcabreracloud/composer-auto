Declarar lo siguiente: 

export PROJECT_ID="ardent-quarter-454807-h6" ; export LOCATION="europe-west2" ; export ZONE="europe-west2-a"   ; export ENVIRONMENT_NAME="composer-environment" ; export SERVICE_ACCOUNT="112344579998-compute@developer.gserviceaccount.com" ; export WORKFLOW_NAME="composer-automation-workflow" ; echo "PROJECT_ID: $PROJECT_ID" ; echo "LOCATION: $LOCATION" ; echo "ZONE: $ZONE" ; echo "WORKFLOW_NAME: $WORKFLOW_NAME" ; export SERVICE_ACCOUNT_EMAIL="112344579998-compute@developer.gserviceaccount.com"


Deployar workflow:
gcloud workflows deploy $WORKFLOW_NAME     --source=workflow-composer-automation.yaml     --location=$LOCATION     --project=$PROJECT_ID


Crear Schedulers:
gcloud scheduler jobs create http composer-start-scheduler     --schedule="30 7 * * *"     --time-zone="Europe/Madrid"     --uri="https://workflowexecutions.googleapis.com/v1/projects/$PROJECT_ID/locations/$LOCATION/workflows/$WORKFLOW_NAME/executions"     --http-method=POST     --headers="Content-Type=application/json"     --message-body='{"argument": "{\"action\": \"start\"}"}'     --oauth-service-account-email="$SERVICE_ACCOUNT_EMAIL"     --location=$LOCATION

gcloud scheduler jobs create http composer-stop-scheduler     --schedule="0 16 * * *"     --time-zone="Europe/Madrid"     --uri="https://workflowexecutions.googleapis.com/v1/projects/$PROJECT_ID/locations/$LOCATION/workflows/$WORKFLOW_NAME/executions"     --http-method=POST     --headers="Content-Type=application/json"     --message-body='{"argument": "{\"action\": \"stop\"}"}'     --oauth-service-account-email="$SERVICE_ACCOUNT_EMAIL"     --location=$LOCATION
