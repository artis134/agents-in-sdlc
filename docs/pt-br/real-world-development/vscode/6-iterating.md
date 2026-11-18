---
title: "Exercício 6 - Itere sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← Lição anterior: Monitorar e gerenciar agentes][previous-lesson] |
| :-- |

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário. Usou o modo agente para adicionar filtragem no cliente e no servidor, o servidor MCP do Playwright para testar manualmente esse trabalho no navegador e, depois, um agente personalizado para implementar recursos de acessibilidade — controles de alternância para os modos de alto contraste e claro —, orientando a sessão em andamento para ampliar o trabalho. Agora é hora de publicar esse trabalho local e revisá-lo como sua equipe faria.

### Cenário

Os fundamentos do design de software e do DevOps não mudam com a inclusão da IA generativa. Ainda é necessário revisar tudo o que o Copilot produz. Vamos enviar as alterações de acessibilidade do Codespace, abrir um pull request e examinar o diff antes de envolver o restante da equipe.

## Publique os recursos de acessibilidade

Os controles para os modos de alto contraste e claro, implementados com o agente personalizado de acessibilidade nos [Exercícios 4][exercise-4] e [5][exercise-5], estão salvos no Codespace como alterações em commits. Vamos enviá-las para uma branch do repositório remoto e abrir um pull request para que a equipe possa revisar.

1. Volte ao Codespace.
2. Abra a visualização **Source Control** no VS Code.
3. Confirme se as alterações de acessibilidade estão em commits. Se ainda houver alterações do Exercício 5 sem commit, prepare-as e faça commit agora com uma mensagem descritiva, como `Add high-contrast and light-mode toggles`.
4. Publique a branch selecionando **Publish Branch** ou usando o menu **...** → **Push**.
5. O VS Code oferecerá a opção de abrir a nova branch no github.com. Aceite ou acesse o repositório manualmente e selecione **Compare & pull request** no banner da branch.
6. Defina um título claro, como `Add high-contrast and light-mode toggles`, e uma breve descrição do que foi feito e por quê.
7. Selecione **Create pull request**.
8. Depois que o PR for aberto, selecione a guia **Files changed** para revisar todo o trabalho. Preste atenção especial a:
   - componentes de interface para alternar entre os modos.
   - uso do armazenamento local para persistir as preferências.
   - alterações de CSS ou estilos para os modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos ARIA e navegação pelo teclado.
   - código JavaScript/TypeScript que controla a alternância entre modos.
9. Volte à guia **Conversation**.
10. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

11. Aguarde a conclusão dos fluxos de trabalho. Se tudo estiver correto, eles serão aprovados.

> [!TIP]
> Quer uma segunda opinião sobre o trabalho de acessibilidade? Mencione `@copilot` em um comentário no PR com uma solicitação, como "revise este PR em busca de outros problemas relacionados às WCAG" ou "sugira melhorias para a navegação pelo teclado". O Copilot iniciará uma nova sessão para responder ao comentário.

## Exercício opcional - continue explorando localmente

Trabalhar de forma iterativa com um agente no IDE é uma habilidade, e a única maneira de desenvolvê-la é praticar. Experimente estas ideias em outras sessões no VS Code:

- Adicione um formulário de interesse de apoiadores à página de detalhes do jogo.
- Implemente paginação na página de lista de jogos.
- Adicione validação de entrada e tratamento de erros aos helpers de acesso a dados em `src/lib/`.
- Amplie o escopo do agente de acessibilidade — por exemplo, audite a ordem de foco do teclado em todo o site.

## Resumo

Parabéns — você concluiu o percurso de VS Code. Neste laboratório, você:

- **Usou o MCP do Playwright para testar manualmente o recurso.** Adicionou o servidor MCP do Playwright e deixou o Copilot controlar um navegador para verificar a filtragem antes de abrir um pull request.
- **Orientou o modo agente em alterações coordenadas na stack.** Adicionou um recurso de filtragem que envolveu o cliente, o servidor e os testes em uma única sessão.
- **Usou um agente personalizado.** Selecionou o agente personalizado de acessibilidade e observou a implementação do modo de alto contraste no repositório.
- **Gerenciou e orientou uma sessão de agente.** Revisou as alterações propostas no editor, aceitou as desejadas e ampliou a sessão com um pedido complementar para o modo claro.
- **Concluiu o fluxo com um pull request.** Publicou o trabalho local e o revisou de ponta a ponta, como sua equipe faria.

## Revisão e próximos passos

Este é o fim do percurso obrigatório de VS Code. Você pode encerrar o workshop aqui.

Se quiser conhecer melhor os recursos de agente do Copilot, os outros percursos abordam cenários relacionados em ambientes diferentes:

- 💻 **[Percurso de CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills, um agente de QA personalizado, MCP do Playwright, Agent Merge e transferências com contexto.
- ☁️ **[Percurso do agente de nuvem](../../cloud/)** — concentre-se em atribuir issues ao agente de nuvem, acompanhar sessões na página de agentes e iterar de forma assíncrona em pull requests.

Você também pode continuar desenvolvendo o que começou aqui. O [awesome-copilot][awesome-copilot] é uma ótima fonte de arquivos de instruções, agentes personalizados e skills que podem ser adaptados aos seus projetos.

Como extensão opcional, [Opcional: Incorpore o Foundry][exercise-7] usa o VS Code e o Microsoft Foundry Toolkit para preparar um modelo, implantar um Backer Concierge e conectá-lo ao site.

## Recursos

- [GitHub Copilot][github-copilot]
- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo agente][agent-mode]

---

| [← Lição anterior: Monitorar e gerenciar agentes][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
