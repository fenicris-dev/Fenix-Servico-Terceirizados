# Fênix Controle de Serviços Terceirizados — v3.6

Padrão: Livro das Regras Fênix v5.2 · Fênix Core v5.2 · Ficha: A, B, E, G, H, I, J
(C cadastro PF/PJ, D ficha de produto e F PDF não marcados nesta versão — ver "Pendências").

## Arquivos
- `fenix-controle-servicos-terceirizados-v3.6.html` — o app (arquivo único).
- `fenix-manual-controle-servicos-terceirizados.md` — manual do usuário (o mesmo texto está dentro do app).

## Como instalar
Substitua o arquivo do app pelo v3.6. O Core vem do endereço fixo
`https://fenicris-dev.github.io/Fenix-Core/fenix-core.js` (carregador oficial, com cópia em cache:
abra o app com internet uma vez para o aparelho guardar o Core).

## O que mudou (resumo)
- Core pelo carregador oficial; selo "v3.6 | c <core>" no topo e no rodapé.
- Sincronia em tempo real entre aparelhos (serviços e clientes): `criarSincronia`, escopo empresa.
  Novos dados ficam em `empresas/{empresaId}/fenix-obras-terceirizadas_sync/…`.
- Migração automática no 1º login: traz os serviços/clientes das coleções antigas
  (`obras_terceirizadas`, `clientes_terceirizados`) e a fila offline antiga para o aparelho, e a sincronia sobe tudo.
  As coleções antigas NÃO são apagadas.
- Login com Google (só aparece com licença definitiva), recuperação de senha pelo Core, teste de 15 dias e modo somente leitura.
- Sobre e Manual pelo Core, Preferências (som, animações reduzidas, tema claro/escuro/automático), Exportar meus dados, Excluir minha conta.
- Segurança: texto escapado (regra 64); backup importado validado (ids seguros) e importado por mesclagem, com confirmação.
- Regra 71: pergunta antes de perder edição (serviço, dia, cliente); sem alert/confirm nativos.
- Código de serviço novo nunca repete um existente.
- Mantidos: chaves do aparelho (`obras_terceirizadas`, `clientes_terceirizados`), `NOME_APP = fenix-obras-terceirizadas` (contador de acessos/backup), cards do Dashboard clicáveis.

## Teste de fumaça (regra 68) — o que foi feito
Rodado em navegador (360 px) com um Core simulado: entrada, migração de dados antigos, texto malicioso exibido como texto
(sem executar), criar serviço, lançar e excluir dia, Sobre/Manual/Preferências, bloqueio em somente leitura, sair da conta.
**Ainda precisa ser feito por você, com o Core real:** (1) entrar com e-mail/senha; (2) abrir em 2 aparelhos e ver a mudança aparecer
nos dois; (3) desligar a internet, editar, religar e ver subir; (4) entrar com Google numa conta de licença definitiva;
(5) conferir a regra do Firestore para `empresas/{empresaId}/fenix-obras-terceirizadas_sync/*` (a regra curinga
`/empresas/{empresaId}/{tipoColecao}/{docId}` com `pertenceAEmpresa` deve cobrir).

## Plano de retorno (regra 70)
A v3.5 continua funcionando: as coleções antigas ficam intactas. Atenção: o que for lançado na v3.6 vai para a coleção nova
e não volta para as antigas; voltar para a v3.5 mostra os dados até o dia da migração.

## Pendências / avisos
- Prints de 360 px no manual (regra 12) ainda não foram gerados: dependem do app rodando com login real.
- Datas das versões antigas no Changelog foram aproximadas pelo histórico das conversas (v3.1 = 09/10/2026 confirmada).
- Grupo C (cadastro de clientes pelo Core, consulta de CNPJ e máscaras), grupo F (PDF dos relatórios) e o uso de
  `dashboard.montar` do Core ficaram para a v3.7; o Dashboard atual (cards clicáveis) foi mantido.
- Dois aparelhos editando o mesmo registro offline: vale a alteração mais recente (sem aviso de conflito).
