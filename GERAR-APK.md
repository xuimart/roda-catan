# Como gerar o APK (Android) — Roda das Estações

O app é um PWA. O APK é uma **casca (TWA)** que abre o seu site publicado em tela
cheia. Por isso:

- Os **sensores (virar o celular)** funcionam no APK, porque a TWA carrega o site
  real via **HTTPS** (o GitHub Pages fornece isso).
- **Quando você atualiza o site, o app atualiza junto.** Você gera o APK só uma vez.
- A TWA é **amarrada à URL** de hospedagem. Então **publique o site primeiro**.

---

## Passo 0 — Publicar o site (obrigatório antes do APK)

Siga o `README.md` (seção "Publicar no GitHub"). No fim você terá uma URL, algo como:

```
https://SEU_USUARIO.github.io/roda-catan/
```

Confirme que ela abre no navegador do celular antes de continuar.

---

## Caminho A — PWABuilder (recomendado, não instala nada)

1. Acesse https://www.pwabuilder.com
2. Cole a URL do seu site publicado
3. Clique em **Start** e depois em **Package for stores → Android**
4. Em "Package options", deixe o padrão (ele já lê o `manifest.json`)
5. Clique em **Download package**

Você recebe um `.zip` com:
- `app-release-signed.apk` → instala direto no celular (ative "fontes desconhecidas")
- `app-release-bundle.aab` → para publicar na Google Play
- `signing.keystore` + `signing-key-info.txt` → **guarde com cuidado**; é a chave
  de assinatura, necessária para publicar atualizações na Play Store.

Para instalar o APK no celular: transfira o `.apk`, abra e confirme a instalação.

---

## Caminho B — Build local com Bubblewrap (avançado)

Requer downloads grandes (JDK + Android SDK). Use só se quiser compilar na máquina.

### 1. Instalar o JDK 17 (uma vez)

```powershell
winget install --id EclipseAdoptium.Temurin.17.JDK -e
```

Feche e reabra o terminal depois de instalar.

### 2. Instalar o Bubblewrap (uma vez)

```powershell
npm install -g @bubblewrap/cli
```

### 3. Inicializar o projeto TWA

```powershell
bubblewrap init --manifest https://SEU_USUARIO.github.io/roda-catan/manifest.json
```

Na primeira execução ele pergunta se pode baixar o Android SDK — aceite. Responda
as perguntas (na dúvida, use os padrões; ele já lê nome, cores e ícones do manifest).

### 4. Compilar

```powershell
bubblewrap build
```

Saída: `app-release-signed.apk` e `app-release-bundle.aab` na pasta.

### 5. Assinatura / Digital Asset Links

Na primeira build o Bubblewrap mostra a "SHA-256 fingerprint" da sua chave. Para o
app abrir **sem a barra de endereço** (modo app puro), publique um arquivo
`.well-known/assetlinks.json` no site com essa fingerprint. O `bubblewrap` mostra o
conteúdo exato a colocar. (Sem isso o app ainda funciona, só mostra uma barrinha do
navegador no topo por 1-2 segundos.)

---

## iPhone (iOS)

TWA é só Android. No iPhone, abra o site no **Safari → Compartilhar → Adicionar à
Tela de Início**. Ele instala como app (PWA) e os sensores funcionam após o toque
no botão "Ativar sensor de virar" (exigência de privacidade da Apple).
