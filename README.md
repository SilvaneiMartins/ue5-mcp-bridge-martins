# Servidor MCP para UE5

[![Testes](https://img.shields.io/badge/testes-87%20aprovados-brightgreen)](tests/) [![Cobertura](https://img.shields.io/badge/cobertura-97%25-brightgreen)](vitest.config.js) [![Node.js](https://img.shields.io/badge/node-%3E%3D18-blue)](package.json) [![Licença: MIT](https://img.shields.io/badge/licen%C3%A7a-MIT-yellow)](LICENSE)

Um servidor MCP (Model Context Protocol) que conecta assistentes de IA ao editor do Unreal Engine 5, permitindo manipular diretamente níveis, atores, Blueprints e Animation Blueprints.

## Por que um repositório independente?

Este servidor MCP fazia originalmente parte do plugin [Martins Unreal Claude](https://github.com/SilvaneiMartins/MartinsUrealClaude), mas foi transferido para um repositório próprio porque:

1. **Escopo ampliado:** o protocolo MCP é um padrão aberto, hoje compatível com vários provedores de IA.
2. **Forks mais simples:** desenvolvedores podem adaptar a ponte para outros serviços de IA sem precisar obter o plugin inteiro do Unreal.
3. **Contribuições da comunidade:** um repositório independente facilita receber contribuições de novos recursos, correções de bugs e suporte a outros clientes MCP.

## Clientes de IA compatíveis

Este servidor usa o padrão [Model Context Protocol](https://modelcontextprotocol.io/) e funciona com **qualquer cliente compatível com MCP**, incluindo:

- **Claude Code** (CLI)
- **Aplicativo Claude Desktop**
- **OpenAI ChatGPT Desktop** (suporte a MCP adicionado em março de 2025)
- **Cursor** (ambiente de desenvolvimento com IA)
- **Replit** (plataforma de programação com IA)
- **Sourcegraph Cody**
- **Windsurf**
- **E qualquer outra ferramenta compatível com MCP**

O protocolo foi [doado à Linux Foundation](https://en.wikipedia.org/wiki/Model_Context_Protocol) em dezembro de 2025 e continua sendo amplamente adotado pela indústria de IA.

---

## Importante: o que esta ferramenta é — e o que não é

**Esta NÃO é uma ferramenta de “texto para jogo”.** Ela não gera modelos 3D, texturas ou animações, nem substitui o trabalho de artistas, animadores e criadores de assets.

**Esta ferramenta acelera o fluxo de trabalho** de quem desenvolve com Unreal Engine. Ela ajuda a:

- **Reduzir tarefas repetitivas de configuração:** criar a estrutura inicial de Blueprints, configurar máquinas de estado e vinculações de entrada.
- **Automatizar tarefas repetitivas no editor:** gerar atores, definir propriedades e organizar níveis usando linguagem natural.
- **Prototipar rapidamente:** testar ideias descrevendo o que você quer, em vez de navegar pelos menus.
- **Aprender UE5:** consultar a documentação integrada da API durante o trabalho.

O objetivo é acelerar as partes mais tediosas do desenvolvimento de jogos para que você possa se concentrar no trabalho criativo que realmente importa.

---

## Requisitos

- **Node.js** 18.0.0 ou superior
- Um endpoint HTTP que implemente a API REST esperada (padrão: `http://localhost:3000`)

> **Observação:** este servidor MCP foi desenvolvido para funcionar com o [plugin Martins Unreal Claude](https://github.com/SilvaneiMartins/MartinsUrealClaude), mas pode ser adaptado para outros back-ends HTTP que implementem a mesma API.

## Início rápido

### 1. Instalação

```bash
git clone https://github.com/SilvaneiMartins/ue5-mcp-bridge-martins.git
cd ue5-mcp-bridge-martins
npm install
```

### 2. Configure seu cliente MCP

Adicione a configuração a seguir à configuração do seu cliente MCP:

**Claude Code** (`~/.claude/settings.json`):
```json
{
  "mcpServers": {
    "unreal": {
      "command": "node",
      "args": ["/path/to/ue5-mcp-bridge-martins/index.js"],
      "env": {
        "UNREAL_MCP_URL": "http://localhost:3000"
      }
    }
  }
}
```

**Claude Desktop** (`claude_desktop_config.json`):
```json
{
  "mcpServers": {
    "unreal": {
      "command": "node",
      "args": ["/path/to/ue5-mcp-bridge-martins/index.js"],
      "env": {
        "UNREAL_MCP_URL": "http://localhost:3000"
      }
    }
  }
}
```

**Outros clientes MCP:** consulte a documentação do seu cliente para configurar um servidor MCP. Este servidor usa transporte stdio.

### 3. Use o servidor

Depois de conectar, você pode interagir com o Unreal Editor usando linguagem natural:

```text
"Crie um Point Light na posição 0, 0, 500"
"Liste todos os StaticMeshActors do nível"
"Crie um novo Actor Blueprint chamado BP_Enemy"
"Adicione uma variável float chamada Health ao BP_Player"
"Crie uma máquina de estado chamada Locomotion no meu Animation Blueprint"
```

---

## Ferramentas disponíveis

### Conexão e contexto

| Ferramenta | Descrição |
|------------|-----------|
| `unreal_status` | Verifica a conexão com o Unreal Editor |
| `unreal_get_ue_context` | Obtém a documentação da API do UE 5.7 por categoria ou consulta |

### Níveis e atores

| Ferramenta | Descrição |
|------------|-----------|
| `unreal_open_level` | Abre, cria ou lista mapas de nível no editor |
| `unreal_spawn_actor` | Gera atores no nível |
| `unreal_get_level_actors` | Lista os atores do nível atual |
| `unreal_set_property` | Define propriedades de atores |
| `unreal_move_actor` | Move, gira ou redimensiona atores |
| `unreal_delete_actors` | Exclui atores do nível |
| `unreal_run_console_command` | Executa comandos do console do Unreal |
| `unreal_get_output_log` | Obtém as entradas recentes do Output Log |

### Assets

| Ferramenta | Descrição |
|------------|-----------|
| `unreal_asset_search` | Pesquisa assets por classe, caminho ou nome |
| `unreal_asset_dependencies` | Obtém todos os assets dos quais um asset depende |
| `unreal_asset_referencers` | Obtém todos os assets que fazem referência a um asset |

### Execução de scripts

| Ferramenta | Descrição |
|------------|-----------|
| `unreal_execute_script` | Executa scripts C++, Python ou de console |
| `unreal_cleanup_scripts` | Remove scripts gerados |
| `unreal_get_script_history` | Obtém o histórico de execução dos scripts |

### Viewport

| Ferramenta | Descrição |
|------------|-----------|
| `unreal_capture_viewport` | Captura uma imagem da viewport ativa |

### Blueprints

| Ferramenta | Descrição |
|------------|-----------|
| `unreal_blueprint_query` | Consulta informações de Blueprints (`list`, `inspect`, `get_graph`) |
| `unreal_blueprint_modify` | Modifica Blueprints: cria, adiciona variáveis e funções, nós e conecta pinos |

### Animation Blueprints

| Ferramenta | Descrição |
|------------|-----------|
| `unreal_anim_blueprint_modify` | Oferece controle completo sobre Animation Blueprints |

### Personagens

| Ferramenta | Descrição |
|------------|-----------|
| `unreal_character` | Consulta e modifica atores `ACharacter` do nível atual |
| `unreal_character_data` | Cria e gerencia DataAssets de configuração de personagens e DataTables de atributos |

### Materiais

| Ferramenta | Descrição |
|------------|-----------|
| `unreal_material` | Cria e atribui instâncias de materiais a Skeletal Meshes |

### Enhanced Input

| Ferramenta | Descrição |
|------------|-----------|
| `unreal_enhanced_input` | Cria e modifica assets do Enhanced Input (`InputAction`, `InputMappingContext`) |

### Fila de tarefas assíncronas

| Ferramenta | Descrição |
|------------|-----------|
| `unreal_task_submit` | Envia uma ferramenta MCP para execução assíncrona em segundo plano |
| `unreal_task_status` | Verifica o estado de uma tarefa assíncrona enviada |
| `unreal_task_result` | Obtém o resultado de uma tarefa concluída |
| `unreal_task_list` | Lista todas as tarefas assíncronas |
| `unreal_task_cancel` | Cancela uma tarefa em execução |

---

## Operações com Animation Blueprints

A ferramenta `unreal_anim_blueprint_modify` oferece controle abrangente sobre Animation Blueprints:

### Operações com máquinas de estado

| Operação | Descrição |
|----------|-----------|
| `get_info` | Obtém uma visão geral da estrutura do AnimBlueprint |
| `get_state_machine` | Obtém informações detalhadas da máquina de estado |
| `create_state_machine` | Cria uma máquina de estado |
| `add_state` | Adiciona um estado à máquina de estado |
| `remove_state` | Remove um estado |
| `set_entry_state` | Define o estado inicial da máquina de estado |
| `connect_state_machine_to_output` | Conecta a máquina de estado à saída de pose do AnimGraph |

### Operações com transições

| Operação | Descrição |
|----------|-----------|
| `add_transition` | Cria uma transição entre estados |
| `remove_transition` | Remove uma transição |
| `set_transition_duration` | Define a duração da mesclagem |
| `set_transition_priority` | Define a prioridade de avaliação |

### Grafo de condições de transição

| Operação | Descrição |
|----------|-----------|
| `add_condition_node` | Adiciona um nó lógico (`TimeRemaining`, `Greater`, `Less`, `And`, `Or`, `Not`, `GetVariable`) |
| `delete_condition_node` | Remove um nó do grafo de transição |
| `connect_condition_nodes` | Conecta nós no grafo de condições |
| `connect_to_result` | Conecta uma condição ao resultado da transição |

### Inspeção de nós e pinos

| Operação | Descrição |
|----------|-----------|
| `get_transition_nodes` | Lista os nós de todos os grafos de transição e seus pinos |
| `inspect_node_pins` | Obtém detalhes dos pinos: tipos, conexões e valores padrão |
| `set_pin_default_value` | Define o valor padrão de um pino com validação de tipo |
| `add_comparison_chain` | Adiciona uma cadeia `GetVariable -> Comparison -> Result` |
| `validate_blueprint` | Retorna o estado da compilação e os diagnósticos |

### Atribuição de animações

| Operação | Descrição |
|----------|-----------|
| `set_state_animation` | Atribui um `AnimSequence`, `BlendSpace`, `BlendSpace1D` ou `Montage` |
| `find_animations` | Pesquisa assets de animação compatíveis |

### Operações em lote

| Operação | Descrição |
|----------|-----------|
| `batch` | Executa várias operações atomicamente |

---

## Sistema de contexto do UE 5.7

O servidor inclui documentação integrada da API do UE 5.7 que pode ser consultada:

### Categorias disponíveis

| Categoria | Descrição |
|-----------|-----------|
| `animation` | Animation Blueprint, máquinas de estado, transições e `UAnimInstance` |
| `blueprint` | Grafos de Blueprint, `UK2Node` e `FBlueprintEditorUtils` |
| `slate` | Widgets Slate, `SNew`/`SAssignNew` e padrões de layout |
| `actor` | Geração de atores, componentes, transformações e iteração |
| `assets` | Carregamento de assets, `TSoftObjectPtr`, `FStreamableManager` e carregamento assíncrono |
| `replication` | Replicação de rede, RPCs, `DOREPLIFETIME` e `OnRep` |
| `enhanced_input` | Sistema Enhanced Input, ações de entrada e contextos de mapeamento |
| `character` | Movimento, configuração e atributos de personagens |
| `material` | Instâncias de materiais, parâmetros e slots |

### Uso

```json
// Obter uma categoria específica
{ "category": "animation" }

// Pesquisar por palavras-chave
{ "query": "state machine transitions" }

// Listar todas as categorias
{}
```

### Injeção automática de contexto

Ative a injeção automática de contexto definindo:

```json
{
  "env": {
    "INJECT_CONTEXT": "true"
  }
}
```

---

## Configuração

| Variável de ambiente | Padrão | Descrição |
|----------------------|--------|-----------|
| `UNREAL_MCP_URL` | `http://localhost:3000` | URL do servidor HTTP do plugin do Unreal |
| `MCP_REQUEST_TIMEOUT_MS` | `30000` | Tempo limite da solicitação, em milissegundos |
| `INJECT_CONTEXT` | `false` | Injeta automaticamente o contexto do UE5 nas respostas das ferramentas |
| `DEBUG` | - | Habilita o registro de depuração |

---

## Solução de problemas

### As ferramentas exibem “NOT CONNECTED”

- Verifique se o back-end HTTP está em execução e acessível.
- Confirme se a variável de ambiente `UNREAL_MCP_URL` aponta para o endpoint correto.
- Confira se o endpoint responde em `/mcp/status`.

### Erros de stdin/stdout

- Verifique se o Node.js 18 ou superior está instalado e disponível no `PATH`.
- Execute `npm install` para instalar as dependências.
- Confirme se o caminho na configuração do cliente MCP aponta para o `index.js` correto.

### Executar a suíte de testes

Este repositório inclui uma suíte de testes Vitest (87 testes) que valida o comportamento da ponte sem precisar de uma instância do Unreal Editor em execução:

```bash
npm install
npm test              # executa todos os testes
npm run test:watch    # executa em modo de observação
npm run test:coverage # executa com relatório de cobertura
```

Os testes simulam o servidor HTTP do Unreal e cobrem a conversão de esquemas, a lógica do cliente HTTP, o carregamento de contexto, a listagem de ferramentas e o roteamento de chamadas.

---

## Contribuição

Contribuições são bem-vindas! Você pode:

- Adicionar suporte a configurações de outros clientes MCP.
- Melhorar a documentação de contexto do UE5.
- Corrigir bugs ou adicionar recursos.
- Enviar melhorias para a documentação.

---

## Licença

MIT com requisito de atribuição — consulte [LICENSE](LICENSE) para obter os detalhes.

**Créditos originais exigidos pela licença:** este projeto deriva do UE5 MCP Server original. A licença exige que os seguintes créditos sejam mantidos:

- Projeto original: UE5 MCP Server
- Repositório original: https://github.com/SilvaneiMartins/ue5-mcp-bridge-martins

**Divulgação exigida:** se você usar este código em outro servidor MCP ou integração de IA, informe esse uso na documentação do seu projeto.

---

## Links relacionados

- [Plugin Martins Unreal Claude](https://github.com/SilvaneiMartins/MartinsUrealClaude) — plugin do Unreal Engine que fornece o back-end HTTP para este servidor MCP.
- [Model Context Protocol](https://modelcontextprotocol.io/) — especificação do protocolo.
- [GitHub do MCP](https://github.com/modelcontextprotocol) — repositórios e SDKs oficiais.
