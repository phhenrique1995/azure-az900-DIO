# Guia de Criação de Máquina Virtual no Azure (AZ-900) - LAB Configurando Recursos e Dimensionamentos em Máquinas Virtuais na Azure

Oi, pessoal! Essa Lab é para o curso da DIO **AZ-900**, e montei este guia pra compartilhar o que aprendi sobre como criar uma máquina virtual (VM) no Azure. Aqui, vou explicar cada aba do processo no portal do Azure, com imagens pra ajudar a visualizar e também comentar um pouco sobre o que é **Área de Trabalho Virtual do Azure** e o **Aplicativo de Funções**.

## Como Criar uma Máquina Virtual no Azure

O processo de criar uma VM no Azure é bem tranquilo, mas tem várias abas pra configurar. Vou te guiar por cada uma delas, com base no que vi no portal e aprendi com as aulas na DIO.

### 1. Básico
Essa é a primeira aba, onde você define as configurações iniciais da sua VM, tipo o nome, sistema operacional, grupo de recursos, usuário e senha.

- **O que fazer**:
  - **Assinatura**: Escolhe a assinatura do Azure que você tá usando (no meu caso, `Azure subscription 1`).
  - **Grupo de recursos**: Cria ou seleciona um grupo de recursos. Eu criei um chamado `AZ-900_LAB_DIO`.
  - **Nome da VM**: Dá um nome pra sua máquina, tipo `machine01`.
  - **Região**: Escolhe uma região próxima, como `(US) East US 2`, pra ter menos latência. No caso usei a mesma do curso
  - **Opções de disponibilidade**: Eu deixei como `Nenhuma redundância de infraestrutura necessária`, já que é só pra teste.
  - **Tipo de segurança**: Deixei o padrão (`Computadores virtuais de inicialização confiável`).
  - **Imagem**: Escolhe o sistema operacional. Eu usei `Windows Server 2019 Datacenter - x64 Gen2`, que já vem com serviços gratuitos qualificados.
  - **Tamanho**: Escolhe o tamanho da VM. Eu fui de `Standard_E2s_v3`, que é bom pra testes.

![Aba Básico](/images/basico.png)

### 2. Discos
Aqui você configura o armazenamento da VM, como o disco do sistema operacional e opções de criptografia. Tirei duas capturas pra mostrar tudo direitinho.

#### Criptografia do Disco
- **O que fazer**:
  - **Criptografia no host**: Eu deixei desmarcado, porque não é necessário pra minha assinatura.
  - **Dica**: Se precisar de mais segurança, dá pra ativar, mas pro meu caso de estudo não precisou.

![Criptografia do Disco](/images/disco1.png)

#### Disco do SO
- **O que fazer**:
  - **Tamanho do disco do SO**: O padrão já veio com 127 GiB, que é suficiente pra um Windows Server.
  - **Tipo de disco do SO**: Escolhi `SSD Premium (armazenamento com redundância local)` pra ter um bom desempenho.
  - **Gerenciamento de chaves**: Deixei como `Chave de criptografia gerenciada pela plataforma`, que é o padrão e já mantém os dados seguros.
  - **Disco de dados**: Não adicionei discos extras, já que é só pra teste.

- **Dica de iniciante 1**: Pra estudos, o `SSD Premium` é uma boa escolha, porque é rápido, mas não é tão caro quanto outras opções.
- **Dica de iniciante 2**: Sempre marque a opção "Excluir com VM", pois no ato de excluir a VM futuramente, também será excluído o IP público. Ou isso ficará gerando cobranças por consumo sem estar usando na prática.
- **Dica de iniciante 3**: Caso você já possua um disco, pode estar vinculando ele na VM sem a necessidade da criação de um novo.

![Disco do SO](/images/disco2.png)

### 3. Rede
Nessa aba, você configura como a VM vai se conectar à internet ou a outros recursos.

- **O que fazer**:
  - **Rede virtual (VNet)**: Criei uma nova chamada `machine01-vnet`.
  - **Sub-rede**: Usei a sub-rede padrão `default (10.0.0.0/24)`.
  - **IP público**: Criei um novo IP público chamado `machine01-ip`.
  - **Grupo de segurança de rede (NSG)**: Escolhi `Básico`, que já cria regras de firewall automaticamente.
  - **Portas de entrada públicas**: Liberei a porta `RDP (3389)` pra acessar a VM via Remote Desktop, já que tô usando Windows.

- **Dica de iniciante 1**: Tem um aviso dizendo que liberar todas as portas deixa a VM vulnerável. Pra testes tá de boa, mas pra produção, melhor usar a aba Avançado e configurar direitinho.
- **Dica de iniciante 2**: Sempre marque a opção "Excluir com VM", pois no ato de excluir a VM futuramente, também será excluído o IP público. Ou isso ficará gerando cobranças por consumo sem estar usando na prática.
- **Dica de iniciante 3**: Caso você já tenha criado uma VNet, mas ela não esteja exibindo, é porque a Região escolhida na primeira tela é divergente da Região usado na criação da sua VNet.

![Aba Rede](/images/rede1.png)

![Aba Rede](/images/rede2.png)

### 4. Gerenciamento
Aqui você configura opções pra gerenciar a VM, como backups e desligamento automático.

- **O que fazer**:
  - **Desligamento automático**: Deixei desativado, porque quero controlar manualmente.
  - **Backup**: Ativei o backup e criei um cofre de Serviços de Recuperação chamado `defaultVault968`.
  - **Subtipo de política**: Escolhi a política `Avançada`, que faz vários backups por dia, com camada operacional de 1 a 30 dias, camada de cofre, camada de instantâneo resiliente de ZRS, e suporte pra início confiável e VMs Confidenciais do Azure.
  - **Política de backup**: Usei uma política chamada `EnhancedPolicy-nhbu085e`. Política essa criada automaticamente ao habilitar o backup. Tendo um menu
  - **Site Recovery**: Deixei desativado, porque não preciso de recuperação de desastres pra esse teste.

- **Dica de iniciante 1**: Ativar o backup é uma boa ideia pra não perder nada importante, mas pra estudos, dá pra pular se quiser economizar.
- **Dica de iniciante 2**: O Desligamento automático é um bom recurso, para redurzir custos em casos de VM que não precisam estar ligadas 24/7, porém lembre-se que ela somente desliga, o acionamento dela para ligar é feito manualmente.
- **Dica de iniciante 3**: Sempre marque a opção "Excluir com VM", pois no ato de excluir a VM futuramente, também será excluído o disco atual. Ou isso ficará gerando cobranças por consumo sem estar usando na prática.
- **Dica de iniciante 4**: Caso você esteja importando uma VM, você pode estar vinculando um disco existente.

![Aba Gerenciamento](/images/gerenciamento.png)

### 5. Monitoramento
Nessa aba, você ativa ferramentas pra monitorar a VM e configurar alertas. Tirei duas capturas: uma pras configurações gerais e outra pra configuração de alertas.

#### Configurações Gerais de Monitoramento
- **O que fazer**:
  - **Alertas**: Ativei as regras de alerta pra receber notificações. Somente para demonstrar as opções disponíveis.
  - **Diagnóstico de inicialização**: Deixei como `Habilitar com a conta de armazenamento gerenciada (recomendado)`.
  - **SO**: Deixei desmarcado o diagnóstico do SO convidado.
  - **Integração do aplicativo**: Deixei desmarcado o monitoramento de integrado do aplicativo.

- **Dica de iniciante**: Ativar o diagnóstico de inicialização é útil pra entender por que a VM não tá iniciando, se der algum problema. Além de ficar sempre alerta do uso e consumo do hardware, podendo atuar de forma proativa para upgrade, não ocasionando problemas futuros, como lentidão ou bugs na aplicação.

#### Configuração de Alertas
- **O que fazer**:
  - Cliquei em **Configurar regras de alerta** e ativei algumas opções padrão, como:
    - Alerta de CPU maior que 80%.
    - Memória disponível menor que 1 GB.
    - Uso de disco maior que 95%.
  - **Notificações**: Configurei pra receber alertas por e-mail (`ph-chagas@hotmail.com`) e também via Resource Manager do Azure.

- **Dica de iniciante**: Configurar alertas é ótimo pra saber se algo tá errado com a VM, tipo se a CPU tá muito alta.

![Configurações Gerais de Monitoramento](/images/monitoramento.png)

### 6. Avançado
Aqui você pode adicionar configurações mais específicas, como extensões ou scripts.

- **O que fazer**:
  - **Extensões**: Tem a opção de selecionar uma extensão pra instalar, como agentes de monitoramento ou scripts. Eu deixei em branco, porque não precisei de nada extra. Mas um exemplo prático, caso deseje criar uma máquina já com o teamviewer, ou alguma ferramenta específica é permitido.
  - **Aplicativos da VM**: Deixei desmarcado, já que não vou instalar aplicativos específicos agora.
  - **Dados personalizados**: Também deixei desmarcado, porque não tô usando scripts ou dados adicionais.

- **Dica de iniciante**: Essa aba é mais pra quem já tá mais avançado e quer automatizar coisas. Pra quem tá começando, dá pra pular.

![Aba Avançado](/images/avançado.png)

### 7. Marcas
As marcas ajudam a organizar os recursos no Azure, tipo etiquetas pra categorizar.

- **O que fazer**:
  - **Nome e valor**: Deixei em branco, mas você pode adicionar tags como `Environment: Test` ou `Project: AZ-900`.
  - No meu caso, o Azure já sugeriu 13 recursos associados, mas não adicionei nenhuma marca manualmente.

- **Dica de iniciante**: Usar marcas é uma boa pra organizar e filtrar recursos, especialmente se você tem muitas VMs.

![Aba Marcas](/images/marcas.png)

### 8. Revisar + Criar
Essa é a última etapa, onde você confere tudo e cria a VM.

- **O que fazer**:
  - O Azure mostra um resumo das configurações que você escolheu.
  - Verifica se a validação passou (tem um ícone verde se tá tudo ok).
  - Clica em **Criar** e espera uns 5-10 minutos pra VM ficar pronta.
  - Depois, pega o IP público da VM pra conectar via RDP (já que é Windows).

- **Dica de iniciante**: Anota o IP público da VM assim que ela for criada, porque você vai precisar dele pra acessar via Remote Desktop.

![Aba Revisar + Criar](/images/revisar-criar.png)

## O que é a Área de Trabalho Virtual do Azure?

A **Área de Trabalho Virtual do Azure** (Azure Virtual Desktop) é um serviço que cria desktops e aplicativos virtuais na nuvem, tipo um Windows 10 ou 11 que você acessa de qualquer dispositivo (celular, notebook, tablet). É como ter seu PC da empresa em qualquer lugar!

### Pra que serve?
- **Trabalho remoto**: Dá pra acessar o desktop da empresa de qualquer lugar.
- **Economia**: Várias pessoas podem usar a mesma VM com sessões separadas, gastando menos.
- **Segurança**: Usa autenticação do Azure e criptografia pra proteger tudo.
- **Personalização**: Instala programas específicos pra equipe (ex.: Visual Studio, Office).
- **Gerenciamento fácil**: Controla tudo pelo portal do Azure.

**Exemplo**: Uma equipe de devs pode usar desktops virtuais com todas as ferramentas configuradas, sem precisar de máquinas potentes em casa.

## O que é o Aplicativo de Funções?

O **Aplicativo de Funções** (Azure Functions) é um serviço de computação sem servidor. Você escreve um código que roda quando algo acontece (um "gatilho"), sem precisar gerenciar servidores.

### Pra que serve?
- **Automatização**: Exemplo: redimensionar imagens automaticamente quando enviadas pro Azure.
- **Escalabilidade**: O Azure ajusta a capacidade conforme a demanda, e você só paga pelo uso.
- **Integração**: Funciona bem com outros serviços do Azure, como bancos de dados.
- **Código rápido**: Suporta Python, JavaScript, C#, e mais, pra criar funções rapidinho.
- **Custo baixo**: Ideal pra tarefas curtas, como enviar notificações ou processar dados.

**Exemplo 1**: Um site pode usar uma função pra converter imagens enviadas pelos usuários pra um tamanho menor, economizando espaço.
**Exemplo 2**: Sempre no ato de criar, ele seleciona automaticamente o sistema operacional recomendado para uso da função. Exmeplo o Python selecionou Linux, já o .NET trá spor padrão o Windows. E caso você mude manualmente, ele exibirá na tela um alerta.

![Aplicativo Funções](/images/funcoes1.png)

![Aplicativo Funções](/images/funcoes2.png)

## Como Usar Este Guia
- **Clone o repositório**: `git clone https://github.com/phhenrique1995/azure-az900-DIO`
- **Veja as imagens**: Elas tão na pasta `images/` do repositório.
- **Siga os passos**: Usa este README como referência pra criar sua VM no Azure.

Ou acesse individualmente:
- [Básico](/images/basico.png)
- [Criptografia do Disco](/images/discos-1.png)
- [Disco do SO](/images/discos-2.png)
- [Rede](/images/rede.png)
- [Gerenciamento](/images/gerenciamento.png)
- [Configurações Gerais de Monitoramento](/images/monitoramento-1.png)
- [Configuração de Alertas](/images/monitoramento-2.png)
- [Avançado](/images/avancado.png)
- [Marcas](/images/marcas.png)
- [Revisar + Criar](/images/revisar-criar.png)

Desenvolvido por [**Pedro Henrique Gomes dos Santos**](https://www.linkedin.com/in/pedro-henrique-gomes-dos-santos/)