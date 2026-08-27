---
name: verificacao-de-regressao
description: 'Transforma um bug relatado em uma verificação de regressão: primeiro cria uma verificação que reproduz o problema e falha, depois corrige o código até ela passar. Use quando for relatado um bug, comportamento incorreto ou alguma funcionalidade que quebrou.'
---

# Verificação de regressão

1. Identifique o comportamento incorreto relatado e transforme o problema em uma verificação automatizada.

2. Crie a verificação antes de alterar o código responsável pelo problema.

3. Execute a verificação e confirme que ela realmente falha por causa do comportamento relatado.

4. Se a verificação passar antes da correção, revise a verificação, pois ela não está reproduzindo corretamente o bug.

5. Faça a menor alteração necessária no código para corrigir o problema.

6. Execute novamente a verificação criada e confirme que ela passou.

7. Execute também a suíte completa de testes para garantir que a correção não quebrou outras funcionalidades.

8. Mantenha a verificação criada no projeto mesmo depois que o bug estiver corrigido. Ela deve continuar protegendo o comportamento contra futuras regressões.

9. Ao verificar erros de uma resposta da API, valide `body.erro.codigo` em vez do texto de `body.erro.mensagem`, pois a mensagem pode mudar enquanto o código representa o tipo do erro.