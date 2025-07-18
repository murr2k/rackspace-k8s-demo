# Rackspace Kubernetes Demo

A modern, interactive Kubernetes demo application showcasing cloud-native deployment on Rackspace's managed Kubernetes platform.

![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)
![Nginx](https://img.shields.io/badge/nginx-%23009639.svg?style=for-the-badge&logo=nginx&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)

## 🚀 Live Demo

**Public URLs**:
- **Custom Domain**: https://linknode.com
- **Direct Access**: http://119.9.118.22:30898
- **Cloudflare Tunnel**: https://daniel-holidays-diesel-gross.trycloudflare.com

**Monitoring Stack**:
- **Grafana Dashboard**: http://119.9.118.22:30300
- **Power Monitor API**: http://119.9.118.22:30500/api/power-data

### What's Special About This Demo?

This entire Kubernetes platform was **built in under 24 hours** using AI-augmented development, showcasing:
- Enterprise-grade infrastructure deployed at record speed
- Production-ready patterns and best practices
- Live auto-scaling and self-healing capabilities
- Professional marketing content integrated seamlessly

Visit the live demo to see both the technical capabilities and learn about AI-augmented development services.

## 📋 Features

- **Modern Web Interface**: Animated gradients, floating particles, and interactive elements
- **Kubernetes Native**: Demonstrates key K8s concepts including:
  - Deployments and ReplicaSets
  - Services (ClusterIP, NodePort, LoadBalancer)
  - ConfigMaps for configuration management
  - Horizontal Pod Autoscaling (HPA)
  - Health checks and probes
  - Resource limits and requests
- **Real-time Power Monitoring**:
  - EAGLE smart meter integration
  - Live power consumption dashboard (Grafana)
  - Time-series data storage (InfluxDB)
  - RESTful API for data ingestion
  - Automatic dashboard updates via CI/CD
- **Secure Access Options**: 
  - SSH tunnel access for development
  - NodePort for cost-effective public access
  - LoadBalancer support (optional)
- **WSL2 Compatible**: Optimized for Windows Subsystem for Linux

## 🛠️ Prerequisites

- Kubernetes cluster (tested on v1.31.1)
- kubectl CLI tool
- Valid kubeconfig file
- Docker (for building custom images)

## 📦 Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/murr2k/rackspace-k8s-demo.git
   cd rackspace-k8s-demo
   ```

2. **Deploy to Kubernetes**
   ```bash
   # Create namespace
   kubectl apply -f k8s/namespace.yaml
   
   # Deploy all resources
   kubectl apply -f k8s/
   ```

3. **Access the application**
   
   For NodePort (public access):
   ```bash
   kubectl get svc -n demo-app
   # Access at http://<NODE-IP>:<NODE-PORT>
   ```
   
   For local access:
   ```bash
   ./wsl-access.sh
   # Access at http://localhost:8090
   ```

## 📁 Project Structure

```
rackspace-k8s-demo/
├── app/                      # Application source code
│   ├── app.py               # Flask application
│   ├── power_monitor.py     # Power monitoring endpoints
│   ├── requirements.txt     # Python dependencies
│   └── Dockerfile           # Container definition
├── k8s/                      # Kubernetes manifests
│   ├── namespace.yaml       # Namespace definition
│   ├── configmap.yaml       # App configuration
│   ├── configmap-nginx.yaml # Nginx HTML content
│   ├── deployment.yaml      # Main deployment
│   ├── deployment-nginx.yaml# Nginx deployment
│   ├── deployment-monitoring.yaml # Monitoring stack
│   ├── service.yaml         # Service definition
│   ├── hpa.yaml            # Horizontal Pod Autoscaler
│   ├── influxdb.yaml       # Time-series database
│   ├── grafana.yaml        # Monitoring dashboard
│   └── grafana-dashboard-*.yaml # Dashboard configs
├── monitoring/              # Monitoring utilities
│   ├── diagnose-no-data.sh # Troubleshooting script
│   └── *.md                # Documentation
├── .github/workflows/       # CI/CD pipelines
│   ├── deploy.yml          # Main deployment
│   └── deploy-monitoring.yml # Monitoring deployment
├── wsl-access.sh           # WSL2 port-forward script
├── ssh-tunnel-setup.sh     # SSH tunnel setup
└── README.md              # This file
```

## 🔧 Configuration

### Service Types

The demo supports three service types:

1. **ClusterIP** (default) - Internal access only
2. **NodePort** - Public access on high port (30000-32767)
3. **LoadBalancer** - Public access with dedicated IP

To change service type:
```bash
kubectl patch svc demo-app-service -n demo-app -p '{"spec":{"type":"NodePort"}}'
```

### Scaling

Manual scaling:
```bash
kubectl scale deployment demo-app -n demo-app --replicas=5
```

Auto-scaling is configured via HPA:
- Min replicas: 2
- Max replicas: 10
- Target CPU: 70%
- Target Memory: 80%

## 🔒 Security

- No default public exposure (ClusterIP)
- SSH-only access option available
- Resource limits enforced
- Non-root container user
- Health checks configured

## 💰 Cost Optimization

- **NodePort**: $0/month (uses existing infrastructure)
- **ClusterIP + Port Forward**: $0/month (most secure)
- **LoadBalancer**: ~$10-25/month (not recommended for demos)

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👤 Author

**Murray Kopit**
- GitHub: [@murr2k](https://github.com/murr2k)
- Email: murr2k@gmail.com

## 🙏 Acknowledgments

- Rackspace for the Kubernetes platform
- The Kubernetes community
- All contributors and testers

## 📊 Power Monitoring Dashboard

This demo includes a real-time power monitoring system integrated with EAGLE smart meters:

### Features
- **Real-time Data**: Updates every 5 seconds
- **Grafana Dashboard**: Beautiful visualizations at http://119.9.118.22:30300
- **REST API**: Accepts JSON data from EAGLE devices
- **Time-series Storage**: InfluxDB for historical data
- **Auto-deployment**: Dashboard updates via CI/CD pipeline

### EAGLE Configuration
Configure your EAGLE device to send data to:
- Protocol: HTTP
- Hostname: 119.9.118.22
- Port: 30500
- Path: /api/power-data

### Dashboard Metrics
- Current power demand (kW)
- Power usage over time
- Min/Max/Average statistics
- Cost rate calculation
- Daily usage projections

## 🚀 CI/CD Pipeline

The project includes GitHub Actions workflows for automated deployment:

1. **Main Deployment** (`deploy.yml`):
   - Deploys application and monitoring stack
   - Updates Grafana dashboards live (no restart)
   - Triggered on push to `main` branch

2. **Monitoring Deployment** (`deploy-monitoring.yml`):
   - Dedicated workflow for monitoring updates
   - Live dashboard updates without service interruption

### Automated Updates
- Push changes to `k8s/grafana-dashboard-*.yaml`
- Pipeline automatically deploys within minutes
- Dashboards update live - just refresh your browser!

---

🌟 If you find this demo helpful, please consider giving it a star on GitHub!
