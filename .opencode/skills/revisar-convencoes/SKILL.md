---
name: revisar-convencoes
description: Revisa uma mudança antes de considerá-la pronta usando uma lista de conferência das convenções, arquitetura, dependências e testes do projeto. Use quando uma implementação estiver terminada, antes de finalizar uma tarefa, criar commit ou considerar uma mudança pronta para entrega.
---

# Revisar convenções

1. Confira o `git diff` e verifique se o `package.json` não recebeu nenhuma dependência nova desnecessária.

2. Verifique a separação das camadas. O controlador não deve importar diretamente um repositório, pois isso indica que uma camada da arquitetura foi ignorada.

3. Confira se a implementação segue as convenções existentes no projeto em vez de criar um padrão diferente sem necessidade.

4. Execute `npm test` e confirme que todos os testes estão passando.

5. Não considere a tarefa concluída apenas porque os testes novos passaram. A suíte completa precisa estar verde.

6. Se algum item da revisão falhar, corrija o problema encontrado.

7. Depois de corrigir qualquer problema, execute novamente toda a lista de conferência desde o primeiro item.

8. Só considere a mudança pronta quando todos os itens da revisão estiverem corretos e todos os testes estiverem passando.