---
name: filtro-em-listagem
description: Adiciona um filtro por query string a uma listagem existente seguindo o padrão de filtros já usado na API. Use quando for necessário adicionar um novo parâmetro de consulta, filtro por campo ou busca em uma rota de listagem como GET /livros.
---

# Filtro em listagem

1. Identifique a rota de listagem que receberá o novo filtro e consulte uma implementação existente que já utilize filtro por query string, como `GET /livros?autor=`.

2. No controlador, leia somente o parâmetro necessário através de `consulta.get('campo')` e repasse esse valor para a camada de serviço.

3. Não coloque a lógica de filtragem no controlador. O serviço é responsável por realizar o filtro.

4. Quando o parâmetro não for informado, mantenha o comportamento original da listagem e retorne todos os registros.

5. Quando o parâmetro for informado, faça a comparação sem diferenciar letras maiúsculas e minúsculas.

6. Para filtros textuais, utilize uma busca com `includes`, seguindo o mesmo comportamento do filtro `?autor=` já existente.

7. Crie verificações para os dois comportamentos: uma chamada sem o filtro e outra chamada com o filtro.

8. Execute as verificações e confirme que a listagem continua funcionando normalmente sem o parâmetro e que o filtro retorna somente os registros esperados quando o parâmetro é utilizado.