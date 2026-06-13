
# 📊 B3 · Controle de Ações & IR

Uma solução **gratuita, privada e de código aberto** para gerenciar suas operações com ações na Bolsa brasileira (B3), calcular o preço médio dos ativos, controlar sua carteira e apurar o Imposto de Renda (IR) mensal para operações de **Swing Trade** com emissão de DARF.

O projeto utiliza uma arquitetura simples e segura: um **App da Web em HTML5/CSS3** (front-end) que se comunica diretamente com uma **Planilha do Google Sheets** através do **Google Apps Script** (back-end), atuando como seu banco de dados privado sem a necessidade de servidores de terceiros.

---

## ✨ Funcionalidades

* 📊 **Dashboard da Carteira:** Visualização do total de ativos, custo total investido e o Lucro/Prejuízo (L/P) acumulado da carteira em tempo real.
* 📋 **Histórico de Operações:** Registro completo e cronológico de todas as compras (C) e vendas (V), com suporte a taxas/emolumentos e identificação da corretora.
* ➕ **Preview de Registro:** Sistema inteligente de visualização antes do salvamento da operação para evitar erros de digitação.
* 🧾 **Cálculo Automatizado de IR (Swing Trade):**
    * Verificação automática do teto de isenção de vendas mensais (R$ 20.000,00).
    * Cálculo automático de Lucro/Prejuízo líquido descontando taxas.
    * Geração dos dados necessários para preenchimento do DARF (Código 6015, valor apurado e mês de vencimento).
* 🔒 **Privacidade Total:** Os dados pertencem exclusivamente a você e ficam armazenados na sua conta do Google Drive.

---

## 🚀 Como Funciona o Sistema?

O ecossistema é dividido em duas partes que trabalham juntas:

1.  **Front-end (`b3-ir-app.html`):** A interface visual onde você interage, adiciona novas operações e consulta seus relatórios tributários.
2.  **Back-end (`Code.gs`):** O script hospedado no Google Apps Script que processa os cálculos matemáticos de Preço Médio (PM) e gera a estrutura correta na sua planilha.

---

## 🛠️ Manual de Configuração e Instalação Passo a Passo

Siga rigorosamente as etapas abaixo para colocar o seu sistema para funcionar de forma personalizada:

### Passo 1: Preparar a Planilha do Google
1. Acesse o seu [Google Drive](https://drive.google.com) com a sua conta do Google.
2. Clique em **Novo** > **Planilhas Google** para criar uma nova planilha em branco.
3. Dê um nome de sua preferência à planilha (ex: `Controle de Investimentos B3`).
4. *Nota importante:* Não é necessário criar colunas ou renomear abas manualmente; o script fará isso sozinho na primeira execução.

### Passo 2: Configurar o Google Apps Script
1. Dentro da planilha que você criou, vá ao menu superior e clique em **Extensões** > **Apps Script**.
2. Apague qualquer código padrão que apareça no editor de texto.
3. Copie o conteúdo integral do arquivo `Code.gs` fornecido e cole no editor do Apps Script.
4. Clique no ícone de **Salvar** (o desenho de um disquete) ou use o atalho `Ctrl + S` (`Cmd + S` no Mac).

### Passo 3: Publicar o Script como App da Web
Para que a interface HTML consiga salvar e ler os dados da planilha, você precisa implantar o script:
1. No canto superior direito da tela do Apps Script, clique no botão **Implantar** (Deploy) e selecione **Nova implantação**.
2. Clique no ícone de engrenagem ao lado de "Selecionar tipo" e escolha **App da Web**.
3. Preencha as configurações exatamente desta forma:
   * **Descrição:** `API Controle B3`
   * **Executar como:** `Eu (seu-email@gmail.com)`
   * **Quem tem acesso:** `Qualquer pessoa` *(Nota de segurança: O uso de JSONP exige essa configuração para cross-origin, mas seus dados continuam protegidos pela URL única do script que só você possui).*
4. Clique no botão **Implantar**.
5. O Google solicitará que você conceda permissões de acesso à sua própria planilha. Clique em **Autorizar acesso**, escolha sua conta Google, vá em *Avançado* e clique em *Acessar Engenharia (não seguro)* ou o nome correspondente para confirmar as permissões.
6. Uma janela com a **URL do App da Web** será gerada. **Copie essa URL** (ela termina com `/exec`).

### Passo 4: Conectar a Interface HTML
1. Abra a página da aplicação (você pode usar a versão hospedada em [ednaldojuni0r.github.io/b3-ir-app/](https://ednaldojuni0r.github.io/b3-ir-app/)).
2. No topo da tela, você verá uma barra amarela solicitando a configuração.
3. Cole a **URL do Apps Script** (copiada no subpasso anterior) no campo de texto indicado.
4. Clique em **Conectar**.
5. O status no cabeçalho mudará para **Conectado** e a barra amarela desaparecerá. Essa configuração fica salva localmente no seu navegador (`localStorage`), eliminando a necessidade de repetir o processo a cada acesso.

---

## 📖 Manual de Operação e Uso Diário

Com a integração concluída, você já pode começar a utilizar as ferramentas do sistema:

### 1. Registrar suas Operações (`➕ Nova Op.`)
* Sempre que realizar uma compra ou venda de ações na B3, acesse esta aba para alimentar o banco de dados.
* Insira a data da nota de corretagem, o ticker do ativo (ex: `PETR4`, `VALE3`), selecione o tipo (**Compra** ou **Venda**), quantidade e o preço unitário praticado.
* **Importante:** Insira o valor total das **Taxas/Emolumentos** daquela operação específica para garantir o cálculo correto do Preço Médio (na Compra, as taxas aumentam seu custo; na Venda, as taxas deduzem seu lucro líquido).
* Confira a caixa de **Preview** para verificar se os dados fazem sentido financeiro e clique em **Registrar operação**. A linha correspondente será adicionada automaticamente à sua planilha do Google Sheets em plano de fundo.

### 2. Acompanhar sua Posição (`📊 Carteira`)
* Esta aba consolida todas as linhas de compra e venda digitadas e calcula o seu **Preço Médio (PM)** de forma ponderada.
* Se você vender todas as ações de um determinado ativo, ele sairá da listagem de "Posição Atual", mas o lucro ou prejuízo histórico gerado por ele continuará computado no card de resumo **Lucro / Prejuízo acumulado**.

### 3. Consultar Histórico (`📋 Operações`)
* Lista todas as operações ordenadas das mais recentes para as mais antigas.
* Se você errou algum dado ao cadastrar, basta clicar no botão vermelho **✕** localizado no final da linha do ativo correspondente para excluir o registro diretamente da planilha.

### 4. Apuração de Imposto de Renda (`🧾 IR / DARF`)
* No encerramento de cada mês corrente, selecione o mês e o ano desejados e clique em **Calcular**.
* O sistema buscará todas as operações classificadas como **Venda (V)** daquele período.
* Se a soma do valor bruto vendido for igual ou inferior a **R$ 20.000,00**, o painel retornará um aviso de **Isento de IR**.
* Se as vendas superarem os R$ 20.000,00 e houver lucro líquido total no mês, o sistema calculará os **15% de imposto devidos** para operações convencionais (Swing Trade) e informará a data limite exata para o pagamento do imposto (último dia útil do mês subsequente).
* Se o resultado final consolidado do mês for negativo, ele informará que houve prejuízo e que você poderá utilizar este saldo para deduzir lucros futuros em sua declaração oficial.

---

## 📝 Regras Fiscais Aplicadas no Sistema

* **Custo de Aquisição:** O preço médio de compra inclui o preço do ativo somado às taxas contratuais da corretora:
  $$\text{Custo Total da Compra} = (\text{Quantidade} \times \text{Preço Unitário}) + \text{Taxas}$$
* **Resultado Líquido de Venda:** O valor líquido recebido na liquidação deduz as taxas operacionais:
  $$\text{Valor Líquido da Venda} = (\text{Quantidade} \times \text{Preço Unitário}) - \text{Taxas}$$
* **Isenção Mensal:** Operações de mercado à vista de ações cujo valor total de alienação (vendas totais) não exceda R$ 20.000,00 dentro do mesmo mês civil são isentas de imposto sobre o lucro.
* **Compensação de Prejuízos:** O código do back-end calcula o lucro de forma segregada por mês civil para base de cálculo tributária do DARF (Código de Receita 6015).

---

## 🛠️ Tecnologias Utilizadas

* **Front-end:** HTML5 nativo, CSS3 estruturado com variáveis de escopo (`:root`), JavaScript Assíncrono Vanilla (ES6+) e comunicação cross-origin via técnica JSONP (dispensando proxies CORS).
* **Back-end & DB:** Google Apps Script (`SpreadsheetApp`, `ContentService`) integrado com Google Sheets Engine.

---

## 📄 Licença

Este projeto está sob a licença MIT. Sinta-se livre para clonar, modificar e distribuir o código conforme as suas necessidades profissionais ou pessoais.
