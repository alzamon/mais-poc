## Bumps on the road
* Vi har ingresser med path og strip 
```    ingress:
      routes:
        - host: foto.utv.k8s.mattilsynet.no
          path: /api
          strip: /api
```
Må finne ut hvordan vi evt. løser same-origin issue. (Løst: Satser på strip i ui-backend nais støtter path)
* Litt vanskelig å vite hva fra [nais dokumentasjonen](https://doc.nais.io) som fungerer i vårt POC-cluster. (Løst: Jeg hadde oversett [denne oversikten]( https://docs.google.com/document/d/1tlfnwvf9UYGH_-VGkXnCcC8YAF6kqYmja_oD4xsDccc/edit))
