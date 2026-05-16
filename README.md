# Sistema IoT de Monitoramento de Temperatura e Umidade do Ar

Projeto de monitoramento ambiental utilizando **NodeMCU ESP8266**, sensor **DHT22** e plataforma **Blynk IoT** para coleta, envio e visualização online de dados de **temperatura** e **umidade relativa do ar**.

Autor: **Prof. Dr. Laerte Peotta de Melo**

---

## 1. Descrição do Projeto

Este projeto tem como objetivo desenvolver um sistema eletrônico simples para medir a **temperatura** e a **umidade do ar** de uma região, disponibilizando os dados em uma plataforma online por meio do **Blynk IoT**.

O sistema realiza leituras periódicas do sensor DHT22, processa os dados no NodeMCU ESP8266 e envia as informações via Wi-Fi para o painel online do Blynk. Os dados podem ser visualizados em tempo real por meio de widgets, medidores e gráficos históricos.

---

## 2. Objetivos

### Objetivo geral

Desenvolver um sistema IoT para monitoramento de temperatura e umidade do ar com publicação online dos dados.

### Objetivos específicos

- Medir temperatura ambiente utilizando o sensor DHT22.
- Medir umidade relativa do ar utilizando o sensor DHT22.
- Enviar os dados coletados para a plataforma Blynk IoT.
- Criar um dashboard online com medidores e gráficos.
- Registrar medições durante pelo menos uma semana.
- Utilizar os dados coletados para análise no relatório final do projeto.

---

## 3. Arquitetura do Sistema

```text
DHT22 → NodeMCU ESP8266 → Wi-Fi → Blynk IoT → Dashboard Online
```

### Fluxo de funcionamento

1. O sensor DHT22 mede temperatura e umidade.
2. O NodeMCU ESP8266 lê os dados do sensor.
3. O ESP8266 conecta-se à rede Wi-Fi.
4. Os dados são enviados para o Blynk IoT.
5. O dashboard apresenta as informações em tempo real e em gráficos históricos.

---

## 4. Lista de Materiais

| Item | Quantidade | Função |
|---|---:|---|
| NodeMCU ESP8266 | 1 | Microcontrolador com Wi-Fi |
| Sensor DHT22 | 1 | Medição de temperatura e umidade |
| Protoboard | 1 | Montagem do circuito sem solda |
| Jumpers macho-macho | Alguns | Conexões na protoboard |
| Jumpers macho-fêmea | Alguns | Conexões entre sensor e placa, se necessário |
| Cabo micro-USB | 1 | Programação e alimentação do NodeMCU |
| Fonte USB 5V | 1 | Alimentação contínua do projeto |
| Caixa plástica ventilada | 1 | Proteção física do circuito |
| Resistor de 10 kΩ | 1 | Pull-up entre VCC e DATA, caso o DHT22 não seja módulo |

---

## 5. Componentes Principais

### NodeMCU ESP8266

O NodeMCU ESP8266 é uma placa de desenvolvimento com conectividade Wi-Fi integrada. Neste projeto, ele é responsável por:

- ler os dados do sensor DHT22;
- conectar-se à rede Wi-Fi;
- enviar os dados para o Blynk IoT;
- executar o código carregado pela Arduino IDE.

### Sensor DHT22

O DHT22 é um sensor digital de temperatura e umidade. Ele foi escolhido por apresentar melhor precisão que o DHT11, sendo mais adequado para coleta de dados ambientais durante vários dias.

---

## 6. Ligações do Circuito

| DHT22 | NodeMCU ESP8266 |
|---|---|
| VCC | 3V3 |
| DATA | D4 |
| GND | GND |

> Observação: caso o DHT22 utilizado seja o sensor de 4 pinos sem placa auxiliar, recomenda-se utilizar um resistor de **10 kΩ** entre **VCC** e **DATA**. Em módulos DHT22 de 3 pinos, esse resistor normalmente já está integrado.

---

## 7. Configuração do Blynk IoT

### 7.1 Criar o Template

No Blynk IoT, acesse:

```text
Zona do Desenvolvedor → Meus Modelos → Novo Modelo
```

Configure:

| Campo | Valor |
|---|---|
| Nome do modelo | Monitoramento Ambiental |
| Hardware | ESP8266 |
| Tipo de conexão | Wi-Fi |

---

### 7.2 Criar os Datastreams

Acesse:

```text
Zona do Desenvolvedor → Meus Modelos → Monitoramento Ambiental → Datastreams
```

Crie dois datastreams do tipo **Pino Virtual**.

#### Datastream 1: Temperatura

| Campo | Valor |
|---|---|
| Nome | Temperatura |
| Apelido | Temperatura |
| Pino virtual | V0 |
| Tipo de dado | Duplo |
| Unidade | Celsius, °C |
| Mínimo | -10 |
| Máximo | 60 |
| Decimais | #.## |
| Histórico | Habilitado |

#### Datastream 2: Umidade

| Campo | Valor |
|---|---|
| Nome | Umidade |
| Apelido | Umidade |
| Pino virtual | V1 |
| Tipo de dado | Duplo |
| Unidade | Percentual, % |
| Mínimo | 0 |
| Máximo | 100 |
| Decimais | #.## |
| Histórico | Habilitado |

---

### 7.3 Criar o dispositivo

Acesse:

```text
Dispositivos → Novo Dispositivo → A partir de um Modelo
```

Selecione o modelo:

```text
Monitoramento Ambiental
```

Depois de criar o dispositivo, copie o **Auth Token** em:

```text
Dispositivo → Informações do Dispositivo → Auth Token
```

---

### 7.4 Criar o Dashboard

No modelo **Monitoramento Ambiental**, acesse:

```text
Painel de Controle da Web
```

Adicione os seguintes widgets:

| Widget | Datastream |
|---|---|
| Medidor de Temperatura | Temperatura / V0 |
| Medidor de Umidade | Umidade / V1 |
| Gráfico de Temperatura | Temperatura / V0 |
| Gráfico de Umidade | Umidade / V1 |

Depois clique em:

```text
Salvar e Aplicar
```

---

## 8. Instalação das Bibliotecas na Arduino IDE

Este projeto utiliza a Arduino IDE para programar o NodeMCU ESP8266.

---

### 8.1 Configurar suporte ao ESP8266

Na Arduino IDE, acesse:

```text
Arquivo → Preferências
```

No campo **URLs adicionais para Gerenciadores de Placas**, adicione:

```text
http://arduino.esp8266.com/stable/package_esp8266com_index.json
```

Depois acesse:

```text
Ferramentas → Placa → Gerenciador de Placas
```

Pesquise por:

```text
esp8266
```

Instale:

```text
esp8266 by ESP8266 Community
```

---

### 8.2 Selecionar a placa

Após a instalação, selecione:

```text
Ferramentas → Placa → NodeMCU 1.0 (ESP-12E Module)
```

Também selecione a porta correta em:

```text
Ferramentas → Porta
```

Exemplo:

```text
COM3
```

---

### 8.3 Bibliotecas necessárias

Na Arduino IDE, acesse:

```text
Sketch → Incluir Biblioteca → Gerenciar Bibliotecas
```

Instale as seguintes bibliotecas:

| Biblioteca | Nome para pesquisar | Finalidade |
|---|---|---|
| Blynk | Blynk | Comunicação com o Blynk IoT |
| DHT sensor library | DHT sensor library by Adafruit | Leitura do sensor DHT22 |
| Adafruit Unified Sensor | Adafruit Unified Sensor | Dependência da biblioteca DHT |

A biblioteca `ESP8266WiFi.h` é instalada junto com o pacote **esp8266 by ESP8266 Community**.

---

## 9. Código do Projeto

> Atenção: não publique senhas reais de Wi-Fi nem o Auth Token do Blynk em repositórios públicos. Use variáveis, arquivo separado ou placeholders.

Crie um arquivo chamado:

```text
monitoramento_ambiental.ino
```

E insira o código abaixo:

```cpp
/*************************************************************
  Projeto: Monitoramento de Temperatura e Umidade
  Placa: NodeMCU ESP8266
  Sensor: DHT22
  Plataforma: Blynk IoT

  Autor: Prof. Dr. Laerte Peotta de Melo
*************************************************************/

/* Dados do Template do Blynk IoT */
#define BLYNK_TEMPLATE_ID "SEU_TEMPLATE_ID"
#define BLYNK_TEMPLATE_NAME "Monitoramento Ambiental"
#define BLYNK_AUTH_TOKEN "SEU_AUTH_TOKEN"

/* Bibliotecas */
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#include <DHT.h>

/* Dados da rede Wi-Fi */
char ssid[] = "NOME_DA_REDE_WIFI";
char pass[] = "SENHA_DA_REDE_WIFI";

/* Configuração do sensor DHT22 */
#define DHTPIN D4        // Pino DATA do DHT22 ligado ao D4 do NodeMCU
#define DHTTYPE DHT22    // Tipo do sensor

DHT dht(DHTPIN, DHTTYPE);

/* Timer do Blynk */
BlynkTimer timer;

/* Função para ler o sensor e enviar os dados ao Blynk */
void enviarDadosSensor()
{
  float umidade = dht.readHumidity();
  float temperatura = dht.readTemperature(); // Temperatura em Celsius

  /* Verifica se houve falha na leitura */
  if (isnan(umidade) || isnan(temperatura)) {
    Serial.println("Erro ao ler o sensor DHT22!");
    return;
  }

  /* Exibe os dados no Monitor Serial */
  Serial.print("Temperatura: ");
  Serial.print(temperatura);
  Serial.print(" °C | Umidade: ");
  Serial.print(umidade);
  Serial.println(" %");

  /* Envia os dados para o Blynk */
  Blynk.virtualWrite(V0, temperatura); // Temperatura no Datastream V0
  Blynk.virtualWrite(V1, umidade);     // Umidade no Datastream V1
}

void setup()
{
  Serial.begin(9600);

  dht.begin();

  /* Conecta ao Wi-Fi e ao Blynk */
  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);

  /*
    Envia os dados a cada 5 minutos.
    5 minutos = 300.000 milissegundos.
  */
  timer.setInterval(300000L, enviarDadosSensor);

  /* Faz uma primeira leitura ao iniciar */
  enviarDadosSensor();
}

void loop()
{
  Blynk.run();
  timer.run();
}
```

---

## 10. Configurações que devem ser alteradas no código

Antes de gravar o código, altere:

```cpp
#define BLYNK_TEMPLATE_ID "SEU_TEMPLATE_ID"
#define BLYNK_AUTH_TOKEN "SEU_AUTH_TOKEN"
```

E também:

```cpp
char ssid[] = "NOME_DA_REDE_WIFI";
char pass[] = "SENHA_DA_REDE_WIFI";
```

---

## 11. Frequência de Atualização

O código envia os dados ao Blynk a cada **5 minutos**:

```cpp
timer.setInterval(300000L, enviarDadosSensor);
```

Essa frequência é adequada para o projeto porque:

- reduz tráfego desnecessário;
- evita sobrecarga no Blynk;
- permite coletar várias medições por hora;
- gera dados suficientes para análise semanal.

---

## 12. Teste do Projeto

Após carregar o código no NodeMCU:

1. Abra o **Monitor Serial** da Arduino IDE.
2. Configure a velocidade para **9600 bps**.
3. Verifique se aparecem leituras como:

```text
Temperatura: 26.40 °C | Umidade: 61.20 %
```

4. Acesse o dashboard do Blynk.
5. Confira se os widgets estão recebendo os valores.

---

## 13. Coleta de Dados para o Relatório

O projeto deve permanecer ligado durante pelo menos **uma semana inteira**.

Durante esse período, recomenda-se registrar:

- data e hora das medições;
- temperatura mínima;
- temperatura máxima;
- temperatura média;
- umidade mínima;
- umidade máxima;
- umidade média;
- prints do dashboard;
- link ou forma de acesso ao painel/plataforma;
- observações sobre o local de instalação.

---

## 14. Estrutura Recomendada do Repositório

```text
monitoramento-ambiental-iot/
├── README.md
├── src/
│   └── monitoramento_ambiental.ino
├── imagens/
│   ├── circuito.png
│   ├── dashboard-blynk.png
│   └── montagem-fisica.png
├── docs/
│   ├── configuracao-blynk.md
│   └── relatorio.md
└── dados/
    └── medicoes-semana.csv
```

---

## 15. Sugestão de Tabela para os Dados

```csv
data,hora,temperatura_c,umidade_percentual
2026-05-01,08:00,24.5,67.2
2026-05-01,08:05,24.6,67.0
2026-05-01,08:10,24.7,66.8
```

---

## 16. Possíveis Erros e Soluções

### Erro: `ESP8266WiFi.h: No such file or directory`

**Causa provável:** pacote ESP8266 não instalado.

**Solução:** instalar o pacote **esp8266 by ESP8266 Community** no Gerenciador de Placas.

---

### Erro: `BlynkSimpleEsp8266.h: No such file or directory`

**Causa provável:** biblioteca Blynk não instalada.

**Solução:** instalar a biblioteca **Blynk** no Gerenciador de Bibliotecas.

---

### Erro: `DHT.h: No such file or directory`

**Causa provável:** biblioteca DHT não instalada.

**Solução:** instalar **DHT sensor library by Adafruit** e **Adafruit Unified Sensor**.

---

### Sensor retorna `nan`

**Causa provável:** ligação incorreta ou falha na leitura.

Verifique:

- se o pino DATA está conectado ao D4;
- se VCC está no 3V3;
- se GND está no GND;
- se o sensor precisa de resistor de 10 kΩ entre VCC e DATA;
- se o sensor utilizado é realmente DHT22.

---

### Dados não aparecem no Blynk

Verifique:

- se o Auth Token está correto;
- se o Wi-Fi está correto;
- se os Datastreams foram criados como V0 e V1;
- se o dispositivo aparece online;
- se o dashboard foi salvo com **Salvar e Aplicar**.

---

## 17. Segurança

Não publique no GitHub:

- senha da rede Wi-Fi;
- Auth Token real do Blynk;
- dados sensíveis de rede.

Use placeholders no código público:

```cpp
#define BLYNK_AUTH_TOKEN "SEU_AUTH_TOKEN"
char ssid[] = "NOME_DA_REDE_WIFI";
char pass[] = "SENHA_DA_REDE_WIFI";
```

---

## 18. Resultados Esperados

Ao final do projeto, espera-se obter:

- sistema funcional de medição de temperatura e umidade;
- envio de dados para o Blynk IoT;
- dashboard online com valores atuais e históricos;
- dados coletados por pelo menos uma semana;
- gráficos de variação de temperatura e umidade;
- relatório contendo análise das medições.

---

## 19. Conclusão

O projeto demonstra a aplicação prática de conceitos de Internet das Coisas, sensores ambientais, comunicação Wi-Fi e visualização online de dados. A solução proposta utiliza componentes de baixo custo e permite acompanhar, em tempo quase real, as condições ambientais de uma região.

Com a coleta contínua durante uma semana, torna-se possível observar variações de temperatura e umidade ao longo do tempo, apoiar análises ambientais simples e disponibilizar informações úteis ao público por meio de uma plataforma online.
