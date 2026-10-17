# Forge

Ferramenta que desenvolvi para configurar, num passo só, um Windows 10 ou 11 recém-formatado: remoção de aplicativos desnecessários, privacidade, energia, políticas de grupo, ajustes do Explorer, segurança, manutenção e instalação de programas.

Ela roda direto do pendrive, em PowerShell 5.1, que já vem com o Windows: não instala nada na máquina para funcionar.

> O código-fonte deste projeto é privado. Aqui estão o que a ferramenta faz e como ela foi pensada.

## Segurança antes de qualquer alteração

Mexer no registro e nas políticas de um computador de outra pessoa pede cuidado. Antes de aplicar qualquer item, a ferramenta:

1. pede elevação de administrador sozinha;
2. cria um ponto de restauração e confere se ele foi criado de fato; se não foi, a aplicação é interrompida;
3. faz cópia das chaves de registro que vai alterar;
4. gera um relatório de antes e depois, que pode ser entregue a quem é dono da máquina;
5. grava um log completo da execução.

Cada ajuste de estado tem reversão. O que não tem volta automática (a remoção de aplicativos que vêm com o Windows, por exemplo) é avisado antes.

## Como se usa

- Prévia sem risco: o modo de demonstração abre tudo com o botão de aplicar desligado, e a prévia mostra o que mudaria sem alterar nada.
- Perfis prontos: padrão, mínimo, computador de uso comum e desempenho. Cada perfil é um arquivo de configuração que dá para copiar e ajustar.
- Vários computadores: um modo silencioso aplica um perfil sem menu e devolve um código de saída por resultado, para orquestrar a preparação em lote.
- Registro de cada execução: perfil, data, máquina e itens aplicados ficam guardados no próprio pendrive, para consulta depois.
- Entrar na BIOS no próximo boot: o firmware fica armado para abrir a configuração na próxima inicialização, útil antes de formatar.

## Interface

Uma interface gráfica em WPF, no formato de assistente: a máquina identificada no topo, as etapas na lateral, um painel com o hardware detectado e as fases do pós-formatação agrupadas, cada uma com uma caixa que marca ou desmarca a fase inteira. O mesmo motor também atende um menu de console.

## Como foi construída

- Motor separado da interface. A interface gráfica, o menu de console e o modo silencioso chamam o mesmo motor, que não depende de tela nenhuma.
- Cada ajuste é uma peça independente. Um ajuste de estado declara como testar, aplicar e reverter; uma ação declara só como executar. Todos trazem nome, categoria, descrição, nível de risco e se vêm marcados por padrão. Acrescentar um ajuste é acrescentar uma peça, sem mexer no resto.
- Parâmetros conferidos. Os ajustes foram comparados com ferramentas consagradas do mercado e com a documentação oficial da Microsoft, e revisados em duas rodadas.
- Autodiagnóstico. Um teste próprio, que só lê e não altera nada, confere a ferramenta antes do uso.

## Tecnologia

PowerShell 5.1 · WPF · winget · registro e políticas de grupo do Windows
