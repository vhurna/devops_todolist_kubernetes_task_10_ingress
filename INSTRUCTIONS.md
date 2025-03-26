nstructions for Validating Changes
This document contains instructions for validating the correct deployment of the MySQL StatefulSet and the application that connects to the database.

1. Validate the MySQL StatefulSet
1.1. Check the StatefulSet Status
Run the following command:

bash
kubectl get statefulsets -n mysql
Expected Output:

The mysql StatefulSet should have 3 replicas (DESIRED, CURRENT, and READY should all be 3).

1.2. Check the MySQL Pods
Run the following command:

bash
kubectl get pods -n mysql
Expected Output:

The pods should have names like mysql-0, mysql-1, and mysql-2.

All pods should be in the Running state.

1.3. Verify MySQL Logs
Check the logs of one of the MySQL pods to ensure there are no errors:

bash
kubectl logs mysql-0 -n mysql
Expected Output:

Look for messages indicating that MySQL has started successfully (e.g., "MySQL initialized" or "Ready for connections").

2. Validate the Application Deployment
2.1. Check the Application Deployment
Run the following command:

bash
kubectl get deployments -n todoapp
Expected Output:

The todoapp deployment should have 1 replica (DESIRED, CURRENT, and READY should all be 1).

2.2. Check the Application Pods
Run the following command:

bash
kubectl get pods -n todoapp
Expected Output:

The pod should be in the Running state.

2.3. Verify Application Logs
Check the logs of the application pod to ensure it has started successfully and connected to the MySQL database:

bash
kubectl logs <todoapp-pod-name> -n todoapp
Expected Output:

Look for messages indicating that the application has started successfully and connected to the MySQL database (e.g., "Connected to MySQL" or "Application started").

2.4. Verify Application Connectivity
Port-forward to the application pod to test connectivity:

bash
kubectl port-forward <todoapp-pod-name> 8080:8080 -n todoapp
Open your browser and navigate to http://localhost:8080. Verify that the application is accessible and functions as expected.

3. Validate the Ingress Configuration
3.1. Check the Ingress Resource
Run the following command:

bash
kubectl get ingress -n todoapp
Expected Output:

The todoapp-ingress should be listed with the correct host and port.

3.2. Test the Ingress Rule
Access the application via the Ingress host (e.g., http://todoapp.example.com). Ensure that the application is accessible and functions as expected.

4. Validate Database Connectivity
4.1. Connect to MySQL from the Application Pod
Run the following command to connect to the MySQL database from the application pod:

bash
kubectl exec -it <todoapp-pod-name> -n todoapp -- mysql -h mysql -u root -p
Expected Output:

You should be able to connect to the MySQL database and execute queries.

5. Cleanup (Optional)
If you want to clean up the resources after validation, run the following commands:

bash
kubectl delete -f deployment.yml -n todoapp
kubectl delete -f service.yml -n todoapp
kubectl delete -f ingress.yml -n todoapp
kubectl delete -f statefulset.yml -n mysql
By following these steps, you can validate that the MySQL StatefulSet, the application, and the Ingress are correctly deployed and functioning as expected.