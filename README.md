# Online Boutique — Kubernetes Deployment

A self-written Kubernetes deployment of Google's [Online Boutique](https://github.com/GoogleCloudPlatform/microservices-demo) 
sample application — 11 microservices in 5 languages (Go, C#, Node.js, 
Python, Java), all wired together manually with custom manifests and 
deployed via GitOps.

> For details on the application itself (what it does, architecture,
> screenshots), see the [original Google repository](https://github.com/GoogleCloudPlatform/microservices-demo).

## What I built

- Wrote all 11 Deployment/Service manifests **from scratch** — Google's
  official images were used, but every manifest, port, and
  environment variable was written by hand, not copied from the
  original repo
- Resolved the full service dependency chain myself (frontend depends
  on 7 other services via environment variables; cartservice depends
  on redis-cart; etc.)
- Deployed via **ArgoCD**, watching this repo with automatic sync —
  no manual `kubectl apply` after initial setup

## Debugging highlights

- **CrashLoopBackOff on frontend** — traced to a missing
  `PRODUCT_CATALOG_SERVICE_ADDR` env var by reading the panic in
  `kubectl logs`
- **Service creation rejected** — `spec.ports: Required value`,
  caused by a Service manifest with no ports (fixed by removing the
  unneeded Service entirely for backend-only services like
  `loadgenerator`)
- **Naming rule violation** — Kubernetes resource names must be
  lowercase; a service named `cartService` failed until renamed to
  `cartservice`
- **gRPC connection timeouts** — `redis-cart` and `currencyservice`
  Services were missing or misconfigured; traced through
  `rpc error: ... i/o timeout` messages in the frontend logs

## Tech stack

Kubernetes · ArgoCD · kubectl



# Online Boutique — Kubernetes Deployment

Google'ın [Online Boutique](https://github.com/GoogleCloudPlatform/microservices-demo) 
örnek uygulamasının, sıfırdan yazılmış Kubernetes deployment'ı — 5 
farklı dilde (Go, C#, Node.js, Python, Java) yazılmış 11 mikroservis, 
elle hazırlanmış manifest'ler ile birbirine bağlandı ve GitOps ile 
deploy edildi.

> Uygulamanın kendisi hakkında detay (ne yaptığı, mimarisi, ekran
> görüntüleri) için [orijinal Google reposuna](https://github.com/GoogleCloudPlatform/microservices-demo) bakabilirsiniz.

## Neler yaptım

- 11 servisin Deployment/Service manifest'lerinin **hepsini sıfırdan** 
  yazdım — Google'ın resmi image'ları kullanıldı, ama her manifest, 
  port ve ortam değişkeni, orijinal repodan kopyalanmadan, elle yazıldı
- Servisler arası tüm bağımlılık zincirini kendim çözdüm (frontend, 
  ortam değişkenleri üzerinden 7 farklı servise bağımlı; cartservice, 
  redis-cart'a bağımlı; vs.)
- **ArgoCD** ile deploy ettim, bu repoyu otomatik senkronizasyonla 
  izliyor — ilk kurulumdan sonra elle `kubectl apply` yapmaya gerek yok

## Karşılaştığım ve çözdüğüm gerçek sorunlar

- **Frontend'in CrashLoopBackOff vermesi** — `kubectl logs`'daki panic 
  mesajından, eksik `PRODUCT_CATALOG_SERVICE_ADDR` ortam değişkenini 
  buldum
- **Service oluşturmanın reddedilmesi** — `spec.ports: Required value`, 
  hiç port tanımlanmamış bir Service'ten kaynaklandı (loadgenerator 
  gibi sadece arka planda çalışan servisler için, Service'i tamamen 
  kaldırarak çözdüm)
- **İsimlendirme kuralı ihlali** — Kubernetes kaynak isimleri küçük 
  harf olmalı; `cartService` isimli bir servis, `cartservice` olarak 
  düzeltilene kadar hata verdi
- **gRPC bağlantı zaman aşımları** — `redis-cart` ve `currencyservice` 
  Service'leri eksik/yanlış yapılandırılmıştı; frontend loglarındaki 
  `rpc error: ... i/o timeout` mesajlarından bunu buldum

## Kullanılan teknolojiler

Kubernetes · ArgoCD · kubectl
