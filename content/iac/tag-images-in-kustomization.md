+++
title = 'Tag Images in Kustomization'
date = 2025-06-04T17:31:58+02:00
draft = false
tags = ["kubernetes", "kustomize"]
+++

## Mettre le numéro de version dans le fichier kustomization.yaml

Pour faciliter la maintenance des applications kubernetes, et avoir toutes les versions
dans le fichier kustomization, on peut utiliser le mot clé newName et newTag pour
remplacer le numéro de versions trouvé dans les fichiers yaml.

Pour être sûr que le numéro de version vient bien du fichier kustomization, je mets un
nom de conteneur bidon dans le déploiement.

```diff
diff --git a/kustomization.yaml b/kustomization.yaml
index 9f4206f..133bf3a 100644
--- a/kustomization.yaml
+++ b/kustomization.yaml
@@ -14,3 +14,11 @@ resources:
   - langfuse-srv-svc.yaml
   - langfuse-pg-sealed-secret.yaml
   - langfuse-srv-sealed-secrets.yaml
+
+images:
+  - name: postgresql
+    newName: postgres
+    newTag: "16"
+  - name: langfuse-server
+    newName: langfuse/langfuse
+    newTag: "3"
diff --git a/langfuse-pg-depl.yaml b/langfuse-pg-depl.yaml
index 6501fff..cb48d47 100644
--- a/langfuse-pg-depl.yaml
+++ b/langfuse-pg-depl.yaml
@@ -19,7 +19,7 @@ spec:
     spec:
       containers:
         - name: langfuse-db
-          image: postgres:16
+          image: postgresql
           env:
             - name: POSTGRES_USER
               value: postgres
diff --git a/langfuse-srv-depl.yaml b/langfuse-srv-depl.yaml
index 74ebb02..be76f64 100644
--- a/langfuse-srv-depl.yaml
+++ b/langfuse-srv-depl.yaml
@@ -20,7 +20,7 @@ spec:
     spec:
       containers:
         - name: langfuse-server
-          image: langfuse/langfuse:2
+          image: langfuse-server
           ports:
             - containerPort: 3000
           env:
```
