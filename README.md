# Capybara AR — Visualizador 3D + Realidade Aumentada via QR Code

## Arquivos
- `main.html` — página principal
- `app.js` — lógica do viewer 3D e da sessão de RA (WebXR)
- `sla.css` — estilos
- `Capybara.stl` — **coloque seu modelo aqui, na mesma pasta**, com esse nome exato

## O que foi corrigido no `app.js` original
- `Capybara.stl.position.x += 0.01` não existia como objeto e quebrava a página inteira (erro no console, nada renderizava). Removido.
- `requestAnimationFrame;` sozinho (sem chamar a função) não fazia nada. O loop de animação correto já existia mais abaixo (`animate()`), então essa linha solta foi removida.
- A câmera agora se ajusta automaticamente ao tamanho real do modelo carregado.

## Como funciona a Realidade Aumentada
Usei **WebXR** (padrão nativo do navegador, sem precisar instalar app):
1. O site pede acesso à câmera do celular e faz *hit-test* — detecta superfícies reais (chão, mesa).
2. Um anel (retícula) mostra onde a capivara vai ser colocada.
3. Você toca na tela e o modelo é posicionado em tamanho real (~35 cm), ancorado no mundo real.
4. Dá para posicionar várias cópias tocando de novo.

**Compatibilidade:**
- ✅ Android com Chrome atualizado (ARCore) — funciona nativamente.
- ❌ iPhone/Safari — a Apple ainda não implementou WebXR no Safari. Para RA no iPhone seria necessário gerar uma versão `.usdz` do modelo e usar AR Quick Look (posso fazer isso depois se você me enviar/gerar um `.glb`/`.usdz` do modelo, ou eu converto o STL se você me der acesso a uma ferramenta de conversão).

## Acesso via QR Code
O botão **"Abrir no celular"** (canto superior direito) abre um painel com um QR Code gerado **na hora**, apontando para a própria URL da página (`window.location.href`). Ou seja: assim que você hospedar o site, o QR já vai funcionar sem precisar gerar nada manualmente.

## Como hospedar (obrigatório para WebXR funcionar)
A câmera e o WebXR só funcionam em **HTTPS** ou em `localhost`. Abrir o `main.html` direto do disco (`file://`) não funciona.

Opções rápidas e gratuitas:
1. **GitHub Pages**: suba a pasta para um repositório e ative Pages nas configurações — já vem com HTTPS.
2. **Netlify Drop** (https://app.netlify.com/drop): arraste a pasta inteira no navegador, gera uma URL HTTPS na hora.
3. **Teste local na sua rede** (para testar no celular sem publicar):
   ```bash
   npx serve .
   # ou
   python3 -m http.server 8000
   ```
   Só que localhost não é acessível pelo celular via IP com HTTPS — para testar no celular de verdade, use a opção 1 ou 2.

Depois de hospedado, abra a URL no computador, clique em "Abrir no celular", escaneie o QR com o celular Android e toque em **"Ver em RA"**.
