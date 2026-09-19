# Roda das Estações — Catan (PWA)

App web (PWA) que sorteia a estação da rodada no Catan girando uma roda.
O recurso sorteado é produzido em dobro até a próxima rodada.

O projeto está pronto para:

1. **Rodar no navegador** (desktop ou celular)
2. **Ser instalado como app** (PWA) direto pelo navegador
3. **Ser publicado no GitHub Pages** com **atualização automática** a cada `git push`
4. **Virar um APK Android** (casca que carrega o site publicado, então também atualiza sozinho)

## Estrutura

```
.
├─ index.html                 # o app
├─ manifest.json              # metadados do PWA (nome, cores, ícones)
├─ sw.js                      # service worker (offline + cache)
├─ .nojekyll                  # evita processamento Jekyll no Pages
├─ icons/
│  ├─ icon-192.png
│  ├─ icon-512.png
│  └─ icon-512-maskable.png
└─ .github/workflows/deploy.yml  # deploy automático no GitHub Pages
```

## 1. Publicar no GitHub (uma vez)

Crie um repositório vazio no GitHub (ex.: `roda-catan`). Depois, aqui na pasta do projeto:

```powershell
git commit -m "PWA Roda das Estações (Catan)"
git remote add origin https://github.com/SEU_USUARIO/roda-catan.git
git push -u origin main
```

Ative o Pages:

1. No GitHub, vá em **Settings → Pages**
2. Em **Build and deployment → Source**, escolha **GitHub Actions**

Pronto. O workflow `deploy.yml` publica o site. O endereço será algo como:

```
https://SEU_USUARIO.github.io/roda-catan/
```

## 2. Atualização automática

Toda vez que você editar o app e fizer:

```powershell
git add .
git commit -m "minha atualização"
git push
```

o GitHub Actions republica o site sozinho. O PWA e o APK carregam sempre a
versão nova (o `sw.js` usa "network-first" para o HTML).

> Dica: se mudar arquivos que ficam em cache (ícones, etc.) e quiser forçar
> atualização imediata em quem já instalou, aumente a versão do cache em `sw.js`
> (ex.: `roda-catan-v1` → `roda-catan-v2`).

## 3. Gerar o APK (Android)

Como o app já é um PWA hospedado, o jeito mais simples é o **PWABuilder**:

1. Acesse https://www.pwabuilder.com
2. Cole a URL do seu site (`https://SEU_USUARIO.github.io/roda-catan/`)
3. Clique em **Package for stores → Android**
4. Baixe o pacote e use o `.apk` (ou `.aab` para publicar na Play Store)

O APK é uma **TWA** (Trusted Web Activity): ele apenas abre o seu site em tela
cheia. Por isso, **quando você atualiza o site, o app atualiza junto** — você não
precisa gerar um APK novo a cada mudança (só se mudar ícone, nome ou permissões).

### Alternativa por linha de comando (Bubblewrap)

```powershell
npm install -g @bubblewrap/cli
bubblewrap init --manifest https://SEU_USUARIO.github.io/roda-catan/manifest.json
bubblewrap build
```

Isso exige o Android SDK/JDK instalados. Para a maioria dos casos, o PWABuilder é
mais rápido.

## Testar localmente

Service workers exigem um servidor (não abra o `index.html` como arquivo direto):

```powershell
python -m http.server 8000
```

Depois abra http://localhost:8000 no navegador.
