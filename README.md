# Kubernetes Manifests



## 🧱 Requisitos

- Docker instalado y en funcionamiento
- Git
- Minikube instalado
- kubectl instalado
- Acceso a una cuenta de GitHub

---

## 📁 Estructura de Carpetas

```bash
minikube-devops/
├── web-static/           # Repositorio con el contenido de la web
└── k8s-manifests/        # Repositorio con los manifiestos de Kubernetes
```

---

## 🚀 Paso a paso

### 1. Fork y clonación del sitio web

1. Ingresar a [https://github.com/ewojjowe/static-website](https://github.com/ewojjowe/static-website)
2. Hacer click en el botón **Fork** y seleccioná tu cuenta.
3. Cloná el fork a tu máquina local:

```bash
cd ~/Documentos/minikube-devops
git clone https://github.com/TU_USUARIO/web-static.git
```

4. Personalizá el contenido HTML/CSS según desees.
5. Versioná los cambios:

```bash
cd web-static
git add .
git commit -m "Personalización del contenido estático"
git push origin main (si main no funciona, probar con master)
```

---

### 2. Crear el repositorio para los manifiestos

1. Crear en GitHub un nuevo repositorio llamado `k8s-manifests`.
2. Clonarlo en la carpeta principal del proyecto:

```bash
cd ~/Documentos/minikube-devops
git clone https://github.com/TU_USUARIO/k8s-manifests.git
```

---

### 3. Iniciar Minikube

```bash
minikube start
```

Podés verificar que el clúcster esté corriendo con:

```bash
kubectl get nodes
```

---

### 4. Crear recursos Kubernetes

Dentro del directorio `k8s-manifests/`, organizá los manifiestos en carpetas:

```bash
mkdir -p persistentvolume persistentvolumeclaim deployment service
```

#### a. PersistentVolume

Archivo: `persistentvolume/pv-web-content.yaml`

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-web-content
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/home/TU_USUARIO/Documentos/minikube-devops/web-static"
```

#### b. PersistentVolumeClaim

Archivo: `persistentvolumeclaim/pvc-web-content.yaml`

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-web-content
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

#### c. Deployment

Archivo: `deployment/web-deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
          volumeMounts:
            - name: web-content
              mountPath: /usr/share/nginx/html
      volumes:
        - name: web-content
          persistentVolumeClaim:
            claimName: pvc-web-content
```

#### d. Service

Archivo: `service/web-service.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-service
spec:
  type: NodePort
  selector:
    app: web
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30036
```

---

### 5. Aplicar los manifiestos

```bash
kubectl apply -f persistentvolume/pv-web-content.yaml
kubectl apply -f persistentvolumeclaim/pvc-web-content.yaml
kubectl apply -f deployment/web-deployment.yaml
kubectl apply -f service/web-service.yaml
```

Verificá que todo esté funcionando:

```bash
kubectl get pods
kubectl get svc
```

---

### 6. Acceder al sitio desde el navegador

Usá el siguiente comando para abrir el sitio:

```bash
minikube service web-service
```

También podés acceder directamente con la URL que se muestra, por ejemplo:

```
http://127.0.0.1:30036
```

---

