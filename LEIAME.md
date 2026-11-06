# Fênix Controle de Serviços Terceirizados — v3.8

Padrão: Livro das Regras Fênix v5.2 · Fênix Core v5.2 · Ficha: A, B, C, E, F, G, H, I, J
(A banco de dados, B login, C cadastro PF/PJ, E backup, F PDF, G dashboard, H login com Google, I tempo real, J tema escuro;
D ficha de produto não se aplica a este app).

## Arquivos
- `fenix-controle-servicos-terceirizados-v3.8.html` — o app (arquivo único, ~610 KB: inclui o gerador de PDF jsPDF 2.5.2, MIT).
- `fenix-manual-controle-servicos-terceirizados.md` — manual do usuário (o mesmo texto está dentro do app).

## Como instalar
Substitua o arquivo do app pelo v3.8. O Core vem do endereço fixo
`https://fenicris-dev.github.io/Fenix-Core/fenix-core.js` (carregador oficial, com cópia em cache).

## O que mudou na v3.8
- **Contratantes sem nome fixo no código:** uma lista única (Carlos Lima, João Carlos e qualquer outro contratante que já exista nos serviços)
  alimenta o filtro da lista, o formulário de serviço e os botões "Resumo" dos relatórios. Testado com um contratante extra.
  Não há tela para cadastrar contratantes: um novo nome só entra por backup/importação.

## O que mudou na v3.7
- **Cadastro de clientes pelo Core (grupo C):** escolha Pessoa Jurídica (CNPJ) ou Pessoa Física (CPF); máscara e validação do
  documento; ao digitar um CNPJ válido o app busca razão social, contato e endereço (`FenixCore.pessoas`, Receita); ao digitar o CEP
  preenche rua, bairro, cidade e UF. Só preenche campos vazios. Telefone, e-mail e endereço completo.
  Cliente com documento válido também é salvo no cadastro central (`pessoas_fisicas` / `pessoas_juridicas`); no app ficam a referência
  e um cache leve (`tipo`, `documento`, contato, endereço), sincronizados entre aparelhos como antes.
  Documento é opcional (clientes antigos ficam "sem documento"); não permite repetir documento nem nome (`obra.cliente` guarda o nome).
- **PDF dos relatórios (grupo F):** botão *Baixar PDF* (serviço, cliente, contratante e geral), gerado no aparelho, sem internet.
  *Imprimir* continua disponível.
- **Aviso de conflito:** se outro aparelho alterar o serviço/cliente que você está editando, aparece um aviso e o app pergunta antes
  de substituir a versão da nuvem. (Continua valendo a última alteração; o aviso só evita sobrescrever sem querer.)
- **Migração dos dados antigos, uma vez por empresa:** a marca fica na nuvem (`empresas/{empresaId}/fenix-obras-terceirizadas_meta/migracao_legado`).
  Se a regra do Firestore negar essa coleção, o app cai no comportamento da v3.6 (marca só no aparelho). Quem já migrou na v3.6
  grava a marca na nuvem no primeiro acesso.
- Mantido tudo da v3.6 (sincronia em tempo real, Google com licença definitiva, tema escuro, Sobre/Manual, regra 71, etc.).

## Teste de fumaça — o que foi feito
Navegador (360 px) com um Core **simulado**: CNPJ válido/inválido/duplicado, CPF, cliente sem documento, nome repetido, CEP,
edição de cliente antigo (endereço em texto livre mantido, renomear propaga para os serviços), aviso e confirmação de conflito
(mudança só de metadado não avisa), 4 PDFs gerados e conferidos em texto, marca de migração (existe / sem permissão / sem internet).
**Ainda precisa ser feito por você, com o Core real:** (1) cadastrar um cliente com CNPJ real e ver os dados virem da Receita;
(2) conferir em `pessoas_juridicas` que o cadastro foi gravado (regra do Firestore); (3) 2 aparelhos: editar o mesmo serviço e ver o aviso;
(4) baixar um PDF no celular; (5) conferir a regra para `empresas/{empresaId}/fenix-obras-terceirizadas_sync/*` e `..._meta/*`.
O layout visual não pôde ser conferido aqui (o Tailwind é bloqueado no ambiente de teste): confira o formulário de cliente em 360 px.

## Plano de retorno (regra 70)
A v3.6 continua funcionando com os mesmos dados da nuvem (os campos novos do cliente são ignorados por ela; o PF aparece sem CNPJ).
A v3.5 mostra os dados só até a migração da v3.6.

## Pendências / avisos
- Prints de 360 px no manual (regra 12) dependem do app rodando com login real.
- Datas das versões antigas no Changelog foram aproximadas pelo histórico das conversas.
- O Dashboard atual (cards clicáveis) foi mantido no lugar de `dashboard.montar` do Core.
- Padronização de texto (regra 42) é aplicada pelo Core ao cadastro central; o nome guardado no app é o digitado (é a chave dos serviços).
