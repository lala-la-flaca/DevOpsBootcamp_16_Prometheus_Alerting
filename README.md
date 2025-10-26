# <img width="250" height="250" alt="image" src="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus/blob/main/Img/1a4f23660b53de848a6a721e3719b077.jpg"/> Module 16 – Monitoring with Prometheus

This exercise is part of Module 16 from the TWN DevOps Bootcamp. In Module 16, we learn how to monitor applications and infrastructure with Prometheus. This module covers everything from installing the Prometheus and Grafana stack on Kubernetes to setting up alerting and visual dashboards. We also learn how to monitor third-party apps such as Redis and collect custom metrics from your own applications. By the end of this module, you know how to build a complete monitoring solution that detects issues early, sends alerts, and visualizes performance data in real time.

---
<a id="demo2"></a>
# 🚨 Demo 2 –  Configure Alerting for our Application
# 📌 Objective
Set up alerting rules and email notifications for application performance issues using Prometheus and Alertmanager.

# 🚀 Technologies Used
* Prometheus: Metrics collection and monitoring system.
* Grafana: Visualization and dashbaord tool.
* Linux: OS.
* Kubernetes: Container Orchestration platform.
* Helm: Package Manager for Kuebrnetes.
* AWS EKS: Managed Kubernetes Cluster.
* Terraform: To deploy K8 infrastructure.

# 🎯 Features
  ✅ Custom alert rules for CPU usage and pod failures.<br>
  📁 Email alerts through Alertmanager.<br>
  ⚙️ Prometheus rule configuration.<br>
  
# Prerequisites
* AWS account with valid keys.
* EKS demo from terraform module.
* Microoservices Demo from kubernetes module.
* Prometheus Demo1
  
# 🏗 Project Architecture
<img src=""/>

# ⚙️ Project Configuration
## Access to Prometheus Web UI and AlertManager
1. Verify the services created in the monitoring namespaces.
   
   ```bash
   kubectl get services - monitoring
   ```
   <img src="" width=800/>
   
2. Access Prometheus Web UI
   Use port forwarding to expose the Prometheus service locally.
   
   ```bash
   kubectl port-forward service/monitoring-kube-prometheus-prometheus 9090:9090  -n monitoring &
   ```
   <img src="" width=800/>
   
3. Access AlertManager Web UI.
   Use port forwarding to expose Alert Manager service locally.
   
   ```bash
    kubectl port-forward service/monitoring-kube-prometheus-alertmanager -n monitoring 9093:9093 &
   ```
   <img src="" width=800/>
   
4. Access Grafana
   Use port forwarding to expose the Grafana service locally.
   ```bash
   kubectl port-forward -n monitoring service/monitoring-grafana 8080:80
   ```
   
 ## Create Prometheus Rules
 5. Access Prometheus Alerts.
    Use port forwarding to expose the Prometheus service locally.

    ```bash
    ```
    <img src="" width=800/>
    
 6. Test the High CPU rule
    ```bash
    ```
    <img src="" width=800/>
    
 7. Create the alert-rules.yaml file using the previous expresion.
    <details><summary><strong>Prometheus Alert Rules K8 Component</strong></summary>
    [PrometheusRule Doc](https://docs.redhat.com/en/documentation/openshift_container_platform/4.13/html/monitoring_apis/prometheusrule-monitoring-coreos-com-v1)
    </details>
  
    ```bash
    ```
    <img src="" width=800/>
  
8. Test the Crash Looping rule.
   ```bash
   ```
   <img src="" width=800/>
   
9. Add the rule to the yaml file.
   
   ```bash
   ```
   <img src="" width=800/>
   
10. Apply the alert-rules.yaml file to the EKS cluster.
   ```bash
   kubectl apply -f alert-rules.yaml
   ```
11. Verify that the PrometheusRules objects are availables in the cluster.
   ```bash
   kubectl get PrometheusRules -n monitoring
   ```

   <img src="" width=800/>
   
12. Verify that the main rules block has been created in the Prometheus Web UI.
   
   <img src="" width=800/>
   
13. Add the CPU dashbaord using the same metric as the CPU High Load.
   <img src="" width=800/> 

## Create the Alert Manager Configuration
14. Create a new yaml file called alert-manager configuration.
   <details><summary><strong>Prometheus Alert Rules K8 Component Doc</strong></summary>
     [Alert Manager Doc](https://docs.redhat.com/en/documentation/openshift_container_platform/4.13/html/monitoring_apis/alertmanagerconfig-monitoring-coreos-com-v1beta1)
    </details>
    
    <details><summary><strong>PrometheusAlert NOK</strong></summary>
        If it does not work, then ensure your CDR version APIversion is the same as your cluster.
        Verify if CDR are available in the cluster
        ```
        kubectl get crd alertmanagerconfigs.monitoring.coreos.com
        ```
        See which API versions the CRD serves (e.g., v1beta1 or v1alpha1)
        ```
         kubectl get crd alertmanagerconfigs.monitoring.coreos.com -o jsonpath='{.spec.versions[*].name}{"\n"}'
       ```
    
       If the CDR  version does not match the yaml, then update the YAML file accordingly.
        ```
        apiVersion: monitoring.coreos.com/v1beta1   # or v1alpha1 if that's what step 1 showed
        kind: AlertmanagerConfig
        ```
  </details>

9. Add the email receivers to the YAML file.
    <details><summary><strong>GMAIL APP PASSWORD</strong></summary>
      Ensure you have two-step authentication activate and the secret is using the APP password for the app.
  </details>

<img src="" width=800/>
    
11. Create a kubernetes secret to store email credentials.
<img src="" width=800/>
12. Apply the secret.
```
kubectl apply -f email-secret.yaml

<img src="" width=800/>

13. Apply the alert-manager configurataton file
    ```
    kubectl apply -f alert-manager.yaml
    ```
    <img src="" width=800/>
    
14. Verify the email receiver configuration in the AlertManeger Web UI

    <img src="" width=800/>

## Testing Alert Manager
15. Create a test pod to strees the system.
    ```bash
    ```
    <img src="" width=800/>
    
17. Verify that the pods is running.
    ```
    kubectl get pods
    ```
    <img src="" width=800/>
    
19. Check your email and confirm that the High CPU alert was triggered and received.
    <img src="" width=800/>


   

