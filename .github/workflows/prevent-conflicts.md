# Conflict Önleme Stratejisi

## Sorun
CI/CD pipeline her deployment'ta image tag'lerini güncelliyor ve bu da manuel değişikliklerle conflict yaratıyor.

## Çözümler

### 1. ArgoCD Image Updater (ÖNERİLEN)
ArgoCD Image Updater kullanarak image tag'lerini git'e yazmadan yönetin.

```yaml
# Deployment'a eklenecek annotation
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    argocd-image-updater.argoproj.io/image-list: myimage=docker.io/myorg/myimage
    argocd-image-updater.argoproj.io/myimage.update-strategy: latest
```

### 2. Branch Stratejisi
- CI/CD sadece `main` branch'e image tag yazsın
- `develop` branch'inde manual çalışın
- Merge ederken `--strategy-option=ours` kullanın

### 3. Kustomize Kullanımı
Base ve overlay yapısı ile image tag'leri ayrı yönetin.

## Şu Anki Durum
- `.gitattributes` ile merge=union stratejisi uygulandı
- Bu sayede otomatik merge daha kolay olacak
