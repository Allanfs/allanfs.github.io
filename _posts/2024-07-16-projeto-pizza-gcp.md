---
layout: post
title:  "Os primeiros passos para o cloud com um projeto real"
date: 2024-07-16 21:46:00 -0300
categories: gcp backend terraform pizzaria
---

Desde o começo do ano venho trabalhando em recursos de tecnologia para a pizzaria.
Nas ultimas semanas foquei no desenvolvimento de uma API que atualiza informações
dos sabores nos diferentes marketplaces em que está disponivel.

Essa API iniciou com a funcionalidade par atualizar o preço de todos os sabores
em todos os tamanhos disponiveis. Essa solução iria trazer grande ganho de tempo
pois até então era um trabalho manual que, apesar dos recursos que o marketplace
dispunha, era necessário fazer algumas tarefas repetidamente várias e várias vezes.

Com uma estrutura simples, cadastrei todos os sabores em um banco de dados local
e organizei suas "forma de venda" em cada lugar. Em uma única linha o usuário
informa o sabor, o preço para cada tamanho (pequena, média, grande e gigante).
Ao fazer o upload desse arquivo o sistema identifica os mapeamentos dele em diferentes
formas e aplicativos, consulta os dados e aplica os novos preços.

Isso foi incrivel!!!

---

Na sequencia, foi hora de trabalhar na disponibilidade dos sabores. No decorrer
do expediente é comum que algum insumo acabe e com isso é preciso desabilitar do
cardapio todos os sabores que fazem uso dele.

Na primeira versão que desenvolvi é realizado apenas a manutenção de um sabor por
vez. E assim como a atualização de preços: isso é incrivel!!

Dado o `id` de um sabor, alterar a sua situação em todas as ocorrencias nos aplicativos
disponiveis.

Novamente, algo que consumia tempo (principalmente durante o expediente) da equipe
e passivel de erro, agora está um passo mais proximo de se deixar de ser uma dor
de cabeça.

Em breve, atualizarei o sistema para inativar todos os sabores relacionados a um
insumo. Aíiii sim será um grande alívio, um clique resolverá tudo e _meu cliente_
poderá focar naquilo que ele faz de melhor: pizza.

---

Enquanto isso, preciso focar em preparar a infraestrutura para comportar o sistema.
Já comecei dockerizando o projeto, a imagem está buildando certinha e fazendo 
upload no Registry do GCP. Fiz um teste simples para executar no `Cloud Run` e 
depois de alguns percalços com o build da imagem (Mac M1 vs Linux amd64), consegui
disponibiliza-la.

Na sequencia, comecei os estudos do Terraform para manter toda a infra versionada
e manutenível. Isso é outra coisa incrível! Muito bom ver tudo alí funcionando sem problemas
e podendo ser inativado ou alterado com alguns comandos.

Com ele, ja crio um banco de dados postgres que já apliquei as migrations do projeto
para estruturar as tabelas. Na sequencia os dados serão populados e poderei
utiliza-la com mais tranquilidade.