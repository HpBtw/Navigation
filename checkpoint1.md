## Descrição do projeto
Este projeto é um aplicativo Android desenvolvido em Kotlin utilizando o framework Jetpack Compose. O foco principal da aplicação é estruturar a navegação entre diferentes telas (como Menu, Perfil e Pedidos) dentro de uma única `Activity` (`MainActivity`), utilizando a biblioteca `Navigation Compose`.

## Objetivo da prova
O objetivo desta avaliação é demonstrar o domínio sobre o roteamento e a navegação em aplicativos Android modernos. A prova avalia a capacidade de configurar o `NavHost`, implementar rotas de navegação e, principalmente, a habilidade de trafegar dados entre as telas utilizando parâmetros obrigatórios, opcionais e múltiplos parâmetros tipados.

## Explicação de cada evolução implementada

- 1. Passagem de Parâmetro Obrigatório (Tela de Perfil)
* **O que foi implementado:** A tela de Perfil foi modificada para ser dinâmica, exigindo e exibindo o nome do usuário recebido da tela anterior.
* **Como a navegação foi configurada:** Na `MainActivity`, a rota da tela de perfil foi atualizada no `NavHost` para incluir o argumento dinâmico diretamente no caminho: `composable(route = "perfil/{nome}")`.
* **Como os parâmetros são enviados e recebidos:**
    * **Envio:** No `MenuScreen`, o evento de clique do botão aciona o `navController.navigate("perfil/Fulano de Tal")`, inserindo o valor do nome na rota.
    * **Recebimento:** Na `MainActivity`, durante a construção da rota, o valor é extraído utilizando `it.arguments?.getString("nome")` (com um fallback para "Usuário Genérico") e passado para os parâmetros da função `@Composable PerfilScreen`.

- 2. Passagem de Parâmetro Opcional (Tela de Pedidos)
* **O que foi implementado:** A tela de Pedidos foi adaptada para receber um parâmetro opcional que identifica o cliente do pedido, permitindo que a tela seja aberta com ou sem essa informação.
* **Como a navegação foi configurada:** A rota foi alterada para utilizar a sintaxe de *query string* (`?chave=valor`), ficando estruturada como `route = "pedidos?cliente={cliente}"`. Além disso, foi definida a lista de argumentos (`arguments = listOf(...)`) usando `navArgument` para estabelecer um valor padrão (`defaultValue = "Cliente Genérico"`).
* **Como os parâmetros são enviados e recebidos:**
    * **Envio:** A navegação ocorre chamando a rota com a *query string*: `navController.navigate("pedidos?cliente=Cliente XPTO")`.
    * **Recebimento:** O dado é recuperado do `NavBackStackEntry` (`it.arguments?.getString("cliente")`) e passado para a `PedidosScreen`. Caso o destino seja chamado apenas como `"pedidos"`, o aplicativo assume o `defaultValue` automaticamente sem causar quebras.

- 3. Passagem de Múltiplos Parâmetros Tipados (Evolução da Tela de Perfil)
* **O que foi implementado:** A tela de Perfil foi expandida para exibir não apenas o nome, mas também a idade do usuário, demonstrando a passagem de múltiplos valores na mesma rota com segurança de tipo.
* **Como a navegação foi configurada:** A rota no `NavHost` foi atualizada para encadear os parâmetros: `route = "perfil/{nome}/{idade}"`. Para garantir a integridade dos dados, a lista de `arguments` foi configurada especificando os tipos estritos usando `NavType.StringType` para o nome e `NavType.IntType` para a idade.
* **Como os parâmetros são enviados e recebidos:**
    * **Envio:** O `MenuScreen` envia os dois parâmetros separados por barra na string de navegação: `navController.navigate("perfil/Fulano de Tal/27")`.
    * **Recebimento:** A `MainActivity` extrai a *String* com `getString("nome")` e o número inteiro com `getInt("idade", 0)`. Ambos os valores são repassados para a `PerfilScreen`, que atualiza a interface de usuário interpolando essas variáveis no componente de texto (`"PERFIL - $nome tem $idade anos"`).
