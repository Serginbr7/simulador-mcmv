# Simulador Minha Casa Minha Vida

Simulador imobiliário desenvolvido para **Sérgio Henrique — Vitta, Franca/SP**. Reúne estimativas de financiamento, composição da entrada e planejamento do fluxo de pagamento em uma página responsiva.

> Simulação ilustrativa. Não substitui a simulação oficial nem a análise de crédito da Caixa. Taxas, subsídios, preços e condições cadastrados precisam ser conferidos antes de cada negociação.

## Funcionalidades

- Simulação nos sistemas PRICE e SAC, com comparação de parcelas e capacidade de financiamento.
- Estimativas de subsídio, uso de FGTS e inclusão de Casa Paulista.
- Prazo ajustado à idade do comprador mais velho e ao período de obra informado.
- Campos para informar financiamento e parcela aprovados pela Caixa.
- Empreendimentos pré-cadastrados: Vitta Parque Sul e Vitta São Vicente.
- Fluxo completo com sinal, blocos de parcelas, periodicidades, documentação, INCC e evolução de obra estimada.
- Resumos para copiar e enviar pelo WhatsApp.
- Exportação de simulação, apresentação do fluxo, proposta detalhada e termos em PDF.
- Tela de acesso com destaques dos empreendimentos e contato pelo WhatsApp.
- Formulário para o cliente preparar um pedido de simulação e encaminhá-lo pelo WhatsApp.
- Layout adaptável ao celular e suporte a tema claro e escuro.

## Tecnologias

| Tecnologia | Uso |
| --- | --- |
| HTML, CSS e JavaScript | Interface e cálculos no navegador |
| jsPDF 2.5.1 | Geração de PDFs |
| jsPDF-AutoTable 3.8.2 | Tabelas dos PDFs |
| Google Fonts | Fontes Figtree e Bricolage Grotesque |
| Web Crypto API | Conferência do hash da senha |
| sessionStorage | Liberação de acesso durante a sessão da aba |
| Clipboard API | Cópia de resumos, com alternativa de seleção manual |
| Web Share API | Compartilhamento de PDFs em celulares compatíveis |

Não depende do Claude. Não exige servidor de aplicação, banco de dados ou etapa de compilação. As bibliotecas PDF e as fontes são carregadas pela internet; esta versão não é totalmente offline.

## Arquivos

| Arquivo | Descrição |
| --- | --- |
| `Simulador_MCMV.html` | Aplicação completa, incluindo estilos, scripts e imagens incorporadas |
| `README.md` | Documentação do projeto |

Para servir a aplicação como página inicial da hospedagem, use o nome `index.html` no lugar de `Simulador_MCMV.html`.

## Executar localmente

Com Python instalado, execute na pasta do projeto:

```bash
python -m http.server 8000
```

Abra `http://localhost:8000/Simulador_MCMV.html`. Caso tenha renomeado o arquivo para `index.html`, abra `http://localhost:8000/`.

Use localhost ou HTTPS. Abrir o arquivo diretamente pode produzir diferenças entre navegadores, principalmente nos recursos de senha, área de transferência e compartilhamento.

## Deploy

1. Renomeie a aplicação para `index.html`.
2. Inclua `index.html` e este `README.md` no repositório.
3. Configure a hospedagem para servir os arquivos estáticos dessa pasta, sem etapa de build.
4. Acesse a URL publicada por HTTPS.
5. Confira o login, uma simulação de referência, a cópia para WhatsApp e cada opção de PDF no ambiente publicado.

Esta aplicação pode ser servida por hospedagens estáticas, como GitHub Pages, Netlify e Vercel.

## Uso

1. Entre com a senha configurada ou utilize a opção de simulação para clientes.
2. Informe renda, nascimento dos compradores e os valores do imóvel.
3. Selecione ou revise subsídio, FGTS, entrada, prazo e sistema de amortização.
4. Consulte o resultado e compare PRICE e SAC.
5. Para detalhar a negociação, selecione **Fluxo de pagamento completo**.
6. Copie o resumo ou exporte o PDF desejado.

Campos de valores aprovados devem ser preenchidos apenas com os dados recebidos da aprovação de crédito. O simulador não consulta a Caixa nem comprova essa aprovação.

## Personalização

### Destaques dos empreendimentos

Na seção `gate-highlights`, localize os elementos `linkParqueSul` e `linkSaoVicente`. Atualmente são cartões informativos com a mensagem “Página em breve”.

Para ativar um cartão, substitua sua tag de abertura `div` por `a`, adicione o endereço em `href`, substitua o fechamento correspondente por `</a>` e atualize o texto de `.property-action` para “Conheça o empreendimento”. Exemplo:

```html
<a class="property-link" id="linkParqueSul"
   href="https://exemplo.com/parque-sul"
   target="_blank" rel="noopener noreferrer">
  <span class="property-tag">Em destaque</span>
  <strong>Vitta Parque Sul</strong>
  <span class="property-detail">Ao lado da UNIFRAN</span>
  <span class="property-action">Conheça o empreendimento</span>
</a>
```

O endereço acima é ilustrativo; substitua pelo link real.

### Contato pelo WhatsApp

Atualize os links `wa.me` do rodapé `.gate-contact`, o telefone exibido e a variável `WHATS` do formulário de clientes. Use o número completo com código do país e DDD, somente com dígitos.

### Senha de acesso

A senha é conferida por SHA-256 da concatenação `SALT + senha`. Para trocá-la, gere o novo hash com o mesmo `SALT` e atualize somente a constante `HASH` do script de acesso. Não publique a senha em texto aberto na documentação.

Uma aba já liberada pode continuar acessando pela sessão existente. Para testar o login novamente, use uma janela anônima ou limpe a chave `mcmv_ok` do sessionStorage.

## Preservação dos cálculos

**Alterações visuais, de login ou de exportação não devem modificar as fórmulas, constantes ou regras da simulação.**

As regras e referências financeiras ficam no próprio HTML. Entre os trechos relevantes estão `faixaDe`, `taxaRef`, `subEst`, `mip`, `calc`, `fxCalc`, as tabelas de subsídio, os índices de INCC e os dados das opções de empreendimentos.

Antes de alterar cálculos ou atualizar referências:

- Registre entradas e resultados de cenários de referência, incluindo PRICE, SAC e fluxo completo.
- Documente a fonte, a data e o motivo da atualização.
- Compare os resultados antes e depois, incluindo arredondamentos, subsídios, seguros, entrada e cronograma.
- Confira os valores apresentados na tela, no resumo e nos PDFs.

Valores estimados, índices históricos e regras embutidas não se atualizam automaticamente. A preservação do código não comprova aderência às condições oficiais vigentes.

## Segurança e dados

- A senha atual é um **bloqueio visual no navegador**, não autenticação segura. O código e as regras podem ser inspecionados por quem acessa a página.
- Não há banco de dados, histórico persistente de clientes ou controle individual de usuários.
- Dados preenchidos são usados no navegador e podem constar nos PDFs e no texto preparado para o WhatsApp. O envio depende da ação do usuário no WhatsApp.
- O carregamento de fontes e bibliotecas utiliza serviços externos.
- Para armazenar cadastros e documentos ou oferecer acesso a vários corretores, implemente autenticação no servidor e permissões de acesso aos dados.

## Verificação antes do uso

- Testar acesso correto e incorreto em uma sessão nova.
- Conferir o formulário de clientes e o destinatário do WhatsApp.
- Conferir resultados financeiros com cenários de referência.
- Testar todos os PDFs no desktop e no celular, incluindo cancelamento de compartilhamento.
- Revisar o layout em telas pequenas e os links dos empreendimentos.

A remoção das dependências do Claude teve verificação de sintaxe e testes simulados de download, compartilhamento, cancelamento e falha. O download real dos PDFs no ambiente publicado ainda precisa de validação.

## Autor

**Sérgio Henrique** — Corretor de imóveis, Vitta, Franca/SP.
