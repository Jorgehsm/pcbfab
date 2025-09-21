### **Introdução**

Este tutorial explica os passos necessários para fabricar uma placa de circuito por meio de usinagem CNC. O processo começa com o design da placa em um software como o KiCad, seguido pela geração do código G usando o FlatCAM, e finaliza com a fabricação da placa usando o software Candle para controlar a máquina CNC.

Para este tutorial, a máquina CNC TwoTrees TTC-3018 S é utilizada. É recomendado ler o tutorial completo antes de começar, pois há avisos importantes.

### **Exportando sua Placa de Circuito**

O foco é a fabricação, não o projeto da placa, então o tutorial assume que você já tem um projeto pronto no KiCad. É importante que a placa tenha sido desenhada na parte de trás (o lado azul no KiCad) para que a orientação de fabricação esteja correta.

O primeiro passo é exportar os arquivos **Gerber (.gbr)** e **Drill (.drl)**, que contêm as trilhas e os furos para os componentes, respectivamente.

#### **Exportando Gerber no KiCad**

Com a PCB aberta no KiCad, siga estes passos:
1.  Vá em **File > Fabrication outputs > Gerbers (.gbr)**.
2.  Na janela que se abre, selecione as camadas **B.Cu** (área de cobre) e **Edge.Cuts** (corte do contorno) em **Include Layers**.
3.  Clique em **"Plot"**.
4.  Não feche a janela, pois ela será usada para gerar os arquivos dos furos.

Este tutorial é para placas de circuito com apenas uma camada de cobre. Para placas com dois lados é necessário importar ambos e o processo se torna um pouco mais complexo.

#### **Exportando Arquivos de Furos no KiCad**

Para exportar os furos, clique em **"Generate Drill Files..."** na mesma janela de plotagem. Na próxima janela, escolha as opções de unidades e formato de arquivo e pressione **"Generate Drill File"**.

Após esses passos, você terá todos os arquivos necessários para a fabricação, e o KiCad não será mais necessário.

### **Configurando e Utilizando o FlatCAM**

O software FlatCAM beta 8.994 é usado para gerar o código G.

1.  Abra o FlatCAM e vá em **File > Open Gerber**.
2.  Selecione e abra os arquivos das trilhas e bordas da placa: `nome-Cu.gbr` e `nome-Edge_Cuts.gbr`.
3.  Em seguida, vá em **File > Open > Open Excellon** e abra o arquivo `nome-PTH.drl` para os furos.
4.  Arraste o mouse sobre os objetos e clique em **Edit > Move to origin** (Shift + O) para mover a placa para a origem.

#### **Configurando Parâmetros**

1.  Vá em **Edit > Preferences**.
2.  Na aba **"General"**, defina a unidade como **mm**.
3.  Na aba **"Excellon"**, mude as unidades para **mm**.
4.  Na aba **"Geometry"**, faça os seguintes ajustes:
    * **Cut Z** (profundidade de corte): $-0.25$.
    * Ative **multi-depth** e defina **depth/pass** como $0.125$.
    * **Travel Z** (altura de viagem da ferramenta): $1$.
    * **Spindle speed**: $9000$.
5.  Clique em **"Apply"** para salvar as mudanças.

#### **Geração do Código G**

---

#### **Isolation Routing (Trilhas)**

Para gerar o código G das trilhas:
1.  Clique duas vezes no arquivo `nome-Cu.gbr` e selecione **"Isolation Routing"**.
2.  Em **Isolation Tool**, defina o diâmetro da ferramenta (**Tool Dia**) como $0.2$.
3.  Clique em **"Generate Geometry"**.
4.  Verifique a geometria gerada com zoom para garantir que não há erros.
5.  Em seguida, clique em **"Generate CNCJob object"**.
6.  Salve o código de CNC.

---

#### **Cutout (Recorte da Borda)**

Para gerar o código G do recorte da placa:
1.  Clique duas vezes no arquivo `nome-Edge_Cuts.gbr` e selecione **"Cutout Tool"**.
2.  Em **Cutout PCB**, defina o **Cut Z** como $-1.8000$ e o **Tool Dia** como $1.0000$.
3.  Clique em **"Generate Geometry"**.
4.  Na aba **Project**, clique duas vezes no arquivo `nome-Edge_Cuts.gbr_cutout` e gere o CNCJob.
5.  Salve o código CNC.

---

#### **Furos**

Para gerar o código G dos furos:
1.  Na aba **Project**, clique duas vezes no arquivo `nome-PTH.drl`.
2.  Abra **"Drilling Tool"** e clique em **"Generate CNCJob object"**.
3.  Salve o código CNC.

Com todos os códigos G gerados, o FlatCAM não é mais necessário.

### **Utilizando a Máquina CNC com Candle**

A máquina CNC TwoTrees TTC-3018 S e o software Candle 1.2b serão usados para controlar o processo.

**Avisos Importantes:**
* Sempre verifique se o probe está conectado ao eixo da fresa e à placa antes de qualquer ação para evitar danos à fresadora.
* Sempre garanta que a área de trabalho da máquina é suficiente para o projeto, caso contrário, a máquina pode ser danificada.
* Não inicie a máquina com o probe conectado à fresa.

#### **Configurando o Candle**

1.  Ligue a máquina CNC e conecte-a ao computador.
2.  Abra o Candle e vá em **Service > Settings**.
3.  Em **"Connection"**, selecione a porta COM à qual a CNC está conectada.

#### **Ferramentas de Usinagem**

Serão usadas duas fresas:
* A **fresa tipo V** para traçar as trilhas da placa.
* A **fresa esférica** para fazer os furos e recortar a placa.

#### **Processo de Fabricação**

Siga a ordem: **Trilhas > Furos > Recorte da Borda**.

1.  **Trilhas**:
    * Instale a fresa tipo V.
    * Abra o código G das trilhas (`nome-Cu.gbr_iso_combined_cnc`) em **File > Open** no Candle.
    * Mova a fresa para a origem desejada da placa usando a função **Jog**.
    * Zere os eixos XY.
    * Conecte um probe à fresa e o outro à placa.
    * Clique em **"Probe-Z"** para a fresa descer e encontrar a superfície da placa.
    * Clique em **"Zero Z"** para zerar o eixo Z.
    * Para placas empenadas, crie um **Heightmap** clicando em **"Create"** na seção **Heightmap**.
    * Na janela do Heightmap, clique em **"Auto"** para definir a área.
    * Clique em **"Probe"** para iniciar o mapeamento.
    * Ao finalizar, salve o mapa clicando em **"Open"**.
    * Na aba Heightmap, clique em **"Edit mode"** e depois em **"Use heightmap"**.
    * Remova o probe da fresa e clique em **"Send"** para começar a fresar as trilhas.

2.  **Furos**:
    * Levante o eixo Z para trocar a fresa para a circular de 1 mm.
    * Abra o arquivo do código G dos furos (`nome-PTH.drl_cnc`).
    * Conecte o probe novamente e realize o probe e zero do eixo Z. Não altere a origem XY.
    * Retire o probe e clique em **"Send"** para começar a perfurar os furos.

3.  **Recorte da Borda**:
    * Abra o código G do recorte da borda (`edge_cuts`).
    * Clique em **"Send"** para recortar a placa.

### **Conclusão**

Após a usinagem, remova a placa, corte as abas que a prendem ao material e use uma esponja de aço (bombril) para remover qualquer excesso de cobre que possa causar curtos-circuitos. Verifique a continuidade das trilhas com um multímetro. Por fim, solde os componentes e estanhe a placa para evitar corrosão.

A fabricação de PCBs por CNC oferece alta precisão e pode ser um processo rápido após a configuração inicial. O sucesso depende de ajustes e experimentação com os parâmetros.

#### **Problemas Comuns e Soluções**

| Problema | Solução |
| :--- | :--- |
| **Trilhas muito finas ou comidas** | Reduza o Cut-Z ou use uma broca com ângulo menor. |
| **Furos danificando as ilhas** | Aumente o tamanho das ilhas no design da placa ou use uma broca em vez de uma fresa. |
| **Recorte de borda ou furos incompletos** | Aumente o Cut-Z da borda e dos furos. |
| **Corte irregular** | Aumente a resolução do mapa de alturas. |
| **Fresa não cortando a placa** | Aumente o Cut
