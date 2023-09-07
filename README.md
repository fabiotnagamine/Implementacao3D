## Configuração do Ambiente

Antes de executar o código Three.js, você precisará configurar o ambiente de desenvolvimento. O código pressupõe que você já possui o Node.js e o npm (Node Package Manager) instalados. Siga estas etapas para configurar o ambiente:

1. **Instalação do Node.js e npm:** Se você ainda não tem o Node.js instalado, faça o download e instale a versão LTS mais recente no site oficial: [Node.js](https://nodejs.org/). Isso também instalará o npm automaticamente.

2. **Instalação do Parcel:** O código utiliza o Parcel para empacotar e servir os arquivos do projeto. Para instalar o Parcel globalmente, execute o seguinte comando no terminal:

   ```bash
   npm install -g parcel-bundler
   ```

### Instruções de Execução

Depois de configurar o ambiente, siga estas instruções para executar o código:

1. **Clonar o Repositório:** Certifique-se de que o código Three.js esteja em um diretório de projeto local ou clone o repositório que contém o código.

2. **Navegar para o Diretório do Projeto:** Abra o terminal e navegue até o diretório do projeto onde o código está localizado.

3. **Instalar Dependências:** Execute o seguinte comando para instalar as dependências do projeto (Three.js, dat.gui, etc.):

   ```bash
   npm install
   ```

4. **Executar o Servidor:** Após a instalação das dependências, inicie o servidor Parcel com o seguinte comando:

   ```bash
   parcel index.html
   ```

5. **Acessar no Navegador:** O servidor Parcel iniciará e exibirá um URL no terminal (geralmente http://localhost:1234). Abra esse URL em seu navegador para visualizar a cena Three.js.

### Visão Geral do Código

Agora, vamos dar uma visão geral do que o código realiza:

- **Importações de Bibliotecas:** O código importa várias bibliotecas Three.js, incluindo o OrbitControls e dat.gui, bem como modelos 3D usando o GLTFLoader.

- **Configuração do Renderizador e Cena:** Ele configura o renderizador WebGL, cria uma cena Three.js e define uma câmera com controles orbitais.

- **Adição de Elementos na Cena:** Diversos elementos são adicionados à cena, como um cubo, um plano, uma esfera e luzes (luz ambiente, luz direcional e luz pontual).

- **Carregamento de Texturas:** O código carrega várias texturas, incluindo texturas para o plano de fundo da cena e texturas para os objetos.

- **Criação de Gui (Interface Gráfica do Usuário):** Utilizando a biblioteca dat.gui, ele cria uma interface de usuário interativa para controlar aspectos da cena, como a cor da esfera, o wireframe, a intensidade das luzes, etc.

- **Animação:** O código inclui uma função animate que é chamada em um loop de renderização. Ela atualiza a cena, realiza animações nos objetos e verifica interações com o mouse.

- **Carregamento de Modelos 3D:** Ele carrega modelos 3D usando o GLTFLoader e adiciona esses modelos à cena. Também inclui botões para iniciar diferentes animações nos modelos.

- **Redimensionamento da Janela:** A janela é redimensionável e o código atualiza a relação de aspecto da câmera e o tamanho do renderizador quando a janela é redimensionada.

Agora você tem uma compreensão geral do que o código faz e como configurar o ambiente para executá-lo. Certifique-se de seguir as etapas acima para configurar o ambiente e experimentar o código Three.js em seu próprio sistema.

### Importações de Bibliotecas

```javascript
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
import * as dat from 'dat.gui';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';
```

Neste passo, são importadas as bibliotecas necessárias para o projeto:

- `THREE`: É a biblioteca Three.js, que fornece a funcionalidade principal para criar gráficos 3D.
- `OrbitControls`: Importado do módulo `three/examples/jsm/controls/OrbitControls.js`, este módulo adiciona controles orbitais para a câmera, permitindo que o usuário interaja com a cena.
- `dat`: É a biblioteca dat.gui, que será usada posteriormente para criar uma interface gráfica do usuário interativa.
- `GLTFLoader`: Importado do módulo `three/examples/jsm/loaders/GLTFLoader.js`, este módulo permite carregar modelos 3D no formato GLTF.

### Configuração do Renderizador e Cena

```javascript
const renderer = new THREE.WebGLRenderer();
renderer.shadowMap.enabled = true;
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

const scene = new THREE.Scene();
```

Neste passo, o renderizador WebGL é configurado. As principais configurações incluem a habilitação das sombras (`shadowMap.enabled`), o redimensionamento do renderizador para corresponder ao tamanho da janela e a adição do elemento do renderizador ao corpo do documento HTML. Uma cena Three.js vazia é criada para conter todos os objetos 3D.

### Criação da Câmera e Controles Orbitais

```javascript
const camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.1, 1000);
const orbit = new OrbitControls(camera, renderer.domElement);

camera.position.set(-10, 30, 30);
```

Aqui, uma câmera com projeção perspectiva é criada e configurada. Os controles orbitais são adicionados à câmera, permitindo que o usuário interaja com a cena. A posição inicial da câmera é definida para (-10, 30, 30).

### Adição de Elementos na Cena

```javascript
const axesHelper = new THREE.AxesHelper(5);
scene.add(axesHelper);

// Criação de objetos como cubo, plano e esfera são codados aqui


const ambientLight = new THREE.AmbientLight(0x333333);
scene.add(ambientLight);

const directionalLight = new THREE.DirectionalLight(0xFFFFFF, 0.8);
// ...
```

Neste passo, vários elementos são adicionados à cena:

- Um sistema de eixos (AxesHelper) é adicionado para fornecer uma referência visual.
- Objetos 3D como um cubo, um plano e uma esfera são criados e adicionados à cena.
- Duas fontes de luz são configuradas: uma luz ambiente (AmbientLight) e uma luz direcional (DirectionalLight).

### Carregamento de Texturas

```javascript
const cubeTextureLoader = new THREE.CubeTextureLoader();
scene.background = cubeTextureLoader.load([
    earth,
    earth,
    earth,
    earth,
    earth,
    earth
]);
```

Aqui, um cubo de textura é criado e usado como plano de fundo da cena. As texturas são carregadas com um CubeTextureLoader, proporcionando um ambiente envolvente.

No código foi pego uma textura de com imagem do planeta terra

### Criação e Configuração de Objetos 3D

```javascript
const box2Geometry = new THREE.BoxGeometry(4, 4, 4);
const box2Material = new THREE.MeshBasicMaterial({ /* ... */ });
const box2 = new THREE.Mesh(box2Geometry, box2Material);
scene.add(box2);


```
Outros objetos 3D, como esferas e modelos 3D, são criados e configurados aqui.

Nesta etapa, objetos 3D, como caixas e esferas, são criados e configurados com geometrias e materiais específicos. Esses objetos são adicionados à cena.

### Animação e Interação

```javascript
function animate(time) {
    *** codigo da animação ***

    renderer.render(scene, camera);
}

renderer.setAnimationLoop(animate);
```

Aqui, uma função de animação é definida e chamada no loop de renderização usando `setAnimationLoop`. Esta função é responsável por atualizar objetos na cena, realizar animações e interagir com o mouse.

### Evento de Redimensionamento de Janela

```javascript
window.addEventListener('resize', function() {
    // O redimensionamento da janela é codado aqui
});
```

Este código lida com o evento de redimensionamento da janela do navegador, atualizando a relação de aspecto da câmera e o tamanho do renderizador para corresponder ao novo tamanho da janela.

### Conclusão

Este é um resumo detalhado do código Three.js fornecido. Ele cria uma cena 3D, carrega modelos, texturas e luzes, permite interações do usuário e animações. Certifique-se de configurar o ambiente conforme as instruções fornecidas para executar o código em seu próprio sistema.# Implementacao3D

