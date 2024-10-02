
<h1  align="center"> React Native Stone Lib </h1>

React Native Stone Lib é uma biblioteca destinada a integrar aplicações React Native com a SDK nativa da Stone, permitindo a comunicação com terminais POS da Stone, como Sunmi, Ingenico ou PAX. A biblioteca possibilita realizar transações financeiras, gerenciar pinpads, imprimir recibos e muito mais.

## 💻 Pré-requisitos

- **NodeJS** >= 18.0.0

- **React Native** >= 0.72

- **Expo** >= 47 (opcional)

- **Token de acesso ao repositório privado da Stone (packageCloud)**

## 🚀 Instalação

A Stone possui um repositório privado no **packageCloud**, portanto, é necessário ter seu token de acesso para baixar as dependências.

### Configuração do Token

Você pode adicionar a variável `StonePos_packageCloudToken` ao seu arquivo `gradle.properties` na pasta `android` do seu projeto ou defini-la como uma variável de ambiente global (útil para builds em CI).

Outras variáveis opcionais:

<table>
    <tbody>
        <tr>
            <td>
                <b>Nome
            </td>
            <td>
                <b>Valor Padrão
            </td>
            <td>
               <b> Descrição
            </td>
        </tr>
        <tr>
            <td>
                StonePos_posMode
            </td>
            <td>
                <b>pos
            </td>
            <td>
                Defina como diferente de <code>pos</code> para remover as dependências do HAL. Útil se estiver construindo para dispositivos móveis que não sejam POS
            </td>
        </tr>
        <tr>
            <td>
                StonePos_includeIngenico
            </td>
            <td>
                <b>true
            </td>
            <td>
                Defina como diferente de <code>true</code> para remover as dependências HAL da Ingenico
            </td>
        </tr>
        <tr>
            <td>
                StonePos_includeSunmi
            </td>
            <td>
                <b>true
            </td>
            <td>
                Defina como diferente de <code>true</code> para remover as dependências HAL da Sunmi
            </td>
        </tr>
        <tr>
            <td>
                StonePos_includeGertec
            </td>
            <td>
                <b>true
            </td>
            <td>
                Defina como diferente de <code>true</code> para remover as dependências HAL da Gertec
            </td>
        </tr>
        <tr>
            <td>
                StonePos_includePositivo
            </td>
            <td>
                <b>true
            </td>
            <td>
                Defina como diferente de <code>true</code> para remover as dependências HAL da Positivo
            </td>
        </tr>
    </tbody>
</table>

### Passos para instalação

Instale a biblioteca usando npm:

```sh
npm install rn-stone-lib
```

### Configuração no Android

#### Passos Obrigatórios

Edite o arquivo `android/app/build.gradle` (NÃO `android/build.gradle`) e adicione:

```gradle
apply from: "../../node_modules/rn-stone-lib/android/stone-repo.gradle"
```

Se estiver desenvolvendo para POS, adicione também:

```gradle
apply from: "../../node_modules/rn-stone-lib/android/dynamic-hal.gradle"
```

Isso habilitará os módulos HAL conforme suas necessidades, desde que `StonePos_include*` esteja definido como `true`.

#### Resolução de Problemas

- **MinSDK**: Defina `minSdkVersion` para 22 no seu arquivo `build.gradle`, conforme exigência da Stone.

- **Erro de mesclagem de manifesto**: Se ocorrer um erro relacionado ao atributo `application@allowBackup`, adicione `tools:replace="android:allowBackup"` ao elemento `<application>` em `android/app/src/main/AndroidManifest.xml`:

```xml
<application

android:name=".MainApplication"

android:label="@string/app_name"

android:icon="@mipmap/ic_launcher"

android:allowBackup="true"

tools:replace="android:allowBackup"

android:theme="@style/AppTheme">

<!-- ... -->

</application>
```

- **Erro 'More than one file was found with OS independent path'**: Adicione ao `android/app/build.gradle`:

```gradle
android {

packagingOptions {

exclude 'META-INF/api_release.kotlin_module'

exclude 'META-INF/client_release.kotlin_module'

  }
}
```

## 📖 Uso

A biblioteca fornece diversas funções para facilitar a integração com os terminais POS da Stone.

### Funções Principais

- `initSDK`: Inicializa o SDK da Stone.

- `activateCode`: Ativa um Stone Code no dispositivo.

- `deactivateCode`: Desativa um Stone Code do dispositivo.

- `getActivatedCodes`: Retorna uma lista de Stone Codes ativados.

- `getAllTransactionsOrderByIdDesc`: Obtém todas as transações ordenadas por ID de forma decrescente.

- `getLastTransaction`: Retorna a última transação realizada.

- `findTransactionWithAuthorizationCode`: Busca uma transação pelo código de autorização.

- `findTransactionWithInitiatorTransactionKey`: Busca uma transação pela chave de transação iniciadora (ATK).

- `findTransactionWithId`: Busca uma transação pelo ID.

- `reversePendingTransactions`: Reverte transações pendentes.

- `voidTransaction`: Cancela uma transação pelo ATK.

- `captureTransaction`: Captura uma transação realizada com `capture = false`.

- `makeTransaction`: Realiza uma transação financeira.

- `cancelRunningTaskMakeTransaction`: Cancela uma transação em andamento.

- `sendTransactionReceiptMail`: Envia o comprovante de transação por e-mail.

- `fetchTransactionsForCard`: Retorna transações registradas para um cartão escaneado.

- `displayMessageInPinPad`: Exibe uma mensagem no PinPad.

- `connectToPinPad`: Conecta-se a um PinPad Bluetooth.

- `printReceiptInPOSPrinter`: Imprime recibo na impressora POS.

- `printHTMLInPOSPrinter`: Imprime conteúdo HTML na impressora POS.

- `mifareDetectCard`: Detecta um cartão Mifare.

- `mifareAuthenticateSector`: Autentica um setor Mifare.

- `mifareReadBlock`: Lê um bloco de um setor Mifare.

- `mifareWriteBlock`: Escreve em um bloco de um setor Mifare.

### Exemplos de Uso

#### Inicializar o SDK da Stone

```javascript
import { initSDK } from 'rn-stone-lib';

async function initializeSDK() {

  try {
    const result = await initSDK('NomeDoSeuApp', 'PIX_KEY', 'PIX_SECRET');

    if (result) {
    console.log('SDK inicializado com sucesso!');
    } else {
    console.log('Falha ao inicializar o SDK.');
    }
  } catch (error) {
    console.error('Erro:', error);
  }

}
```

#### Ativar um Stone Code

``` javascript
import { activateCode } from  'rn-stone-lib';

async  function  activateStoneCode() {

try {
  const  success = await  activateCode('SEU_STONE_CODE');

  if (success) {
    console.log('Stone Code ativado com sucesso!');
  } else {
    console.log('Falha ao ativar o Stone Code.');
  }
  } catch (error) {
    console.error('Erro:', error);
  }

}
```

#### Desativar um Stone Code

```javascript
import { deactivateCode } from 'rn-stone-lib';

async function deactivateStoneCode() {

  try {
    const success = await deactivateCode('SEU_STONE_CODE');

    if (success) {
      console.log('Stone Code desativado com sucesso!');
    } else {
      console.log('Falha ao desativar o Stone Code.');
    }
  } catch (error) {
    console.error('Erro:', error);
  }

}
```

#### Obter Stone Codes Ativados

```javascript
import { getActivatedCodes } from 'rn-stone-lib';

async function fetchActivatedCodes() {

  try {
    const codes = await getActivatedCodes();

    console.log('Stone Codes ativados:', codes);
  } catch (error) {
    console.error('Erro ao obter Stone Codes ativados:', error);
  }

}
```

#### Obter Todas as Transações Ordenadas por ID Decrescente

```javascript
import { getAllTransactionsOrderByIdDesc } from 'rn-stone-lib';

async function fetchAllTransactions() {
  try {
    const transactions = await getAllTransactionsOrderByIdDesc();
    console.log('Transações:', transactions);
  } catch (error) {
    console.error('Erro ao obter transações:', error);
  }
}
```

#### Obter a Última Transação Realizada

```javascript
import { getLastTransaction } from 'rn-stone-lib';

async function fetchLastTransaction() {

  try {
    const transaction = await getLastTransaction();

    if (transaction) {
      console.log('Última transação:', transaction);
    } else {
      console.log('Nenhuma transação encontrada.');
    }
  } catch (error) {
    console.error('Erro ao obter a última transação:', error);
  }

}
```

#### Buscar Transação pelo Código de Autorização

```javascript
import { findTransactionWithAuthorizationCode } from 'rn-stone-lib';

async function findTransactionByAuthorizationCode(authCode) {

  try {
    const transaction = await findTransactionWithAuthorizationCode(authCode);

    if (transaction) {
      console.log('Transação encontrada:', transaction);
    } else {
      console.log('Transação não encontrada.');
    }
  } catch (error) {
    console.error('Erro ao buscar transação:', error);
  }

}
```

#### Buscar Transação pela Chave de Transação Iniciadora (ATK)

```javascript
import { findTransactionWithInitiatorTransactionKey } from 'rn-stone-lib';

async function findTransactionByATK(atk) {

  try {
    const transaction = await findTransactionWithInitiatorTransactionKey(atk);

    if (transaction) {
      console.log('Transação encontrada:', transaction);
    } else {
      console.log('Transação não encontrada.');
    }
  } catch (error) {
    console.error('Erro ao buscar transação:', error);
  }

}
```

#### Buscar Transação pelo ID

```javascript
import { findTransactionWithId } from 'rn-stone-lib';

async function findTransactionById(id) {

  try {
    const transaction = await findTransactionWithId(id);

    if (transaction) {
      console.log('Transação encontrada:', transaction);
    } else {
      console.log('Transação não encontrada.');
    }
  } catch (error) {
    console.error('Erro ao buscar transação:', error);
  }

}
```

#### Reverter Transações Pendentes

```javascript
import { reversePendingTransactions } from 'rn-stone-lib';

async function reversePending() {
  try {
    const success = await reversePendingTransactions();

    if (success) {
      console.log('Transações pendentes revertidas com sucesso!');
    } else {
      console.log('Nenhuma transação pendente para reverter.');
    }
  } catch (error) {
    console.error('Erro ao reverter transações pendentes:', error);
  }
}
```

#### Cancelar uma Transação

```javascript
import { voidTransaction } from 'rn-stone-lib';

async function cancelTransaction(atk) {

  try {
    const transaction = await voidTransaction(atk);

    console.log('Transação cancelada:', transaction);
  } catch (error) {
    console.error('Erro ao cancelar transação:', error);
  }

}
```

#### Capturar uma Transação

```javascript
import { captureTransaction } from 'rn-stone-lib';

async function captureTransactionByATK(atk) {

  try {
    const transaction = await captureTransaction(atk);

    console.log('Transação capturada:', transaction);
  } catch (error) {
    console.error('Erro ao capturar transação:', error);
  }

}
```

#### Realizar uma Transação

```javascript
import { makeTransaction } from  'rn-stone-lib';

async function processTransaction() {

  try {
    const  transaction = await  makeTransaction({
    amount:  5000, // Valor em centavos (R$50,00)
    installmentCount:  1,
    installmentHasInterest:  false,
    capture:  true,
    });

    console.log('Transação realizada:', transaction);
  } catch (error) {
    console.error('Erro na transação:', error);
  }

}
```

#### Cancelar uma Transação em Andamento

```javascript
import { cancelRunningTaskMakeTransaction } from 'rn-stone-lib';

async function cancelCurrentTransaction() {
  try {
    const success = await cancelRunningTaskMakeTransaction();
    if (success) {
      console.log('Transação em andamento cancelada com sucesso!');
    } else {
      console.log('Nenhuma transação em andamento para cancelar.');
    }
  } catch (error) {
    console.error('Erro ao cancelar transação:', error);
  }

}
```

#### Enviar Comprovante de Transação por E-mail

```javascript
import { sendTransactionReceiptMail } from 'rn-stone-lib';

async function sendReceiptByEmail(atk) {
  try {
    const success = await sendTransactionReceiptMail(
      atk,
      'CLIENT', // ou 'MERCHANT'
      [{ email: 'cliente@example.com', name: 'Cliente' }],
      { email: 'loja@example.com', name: 'Sua Loja' }
    );
    if (success) {
      console.log('Comprovante enviado por e-mail com sucesso!');
    } else {
      console.log('Falha ao enviar o comprovante.');
    }
  } catch (error) {
    console.error('Erro ao enviar comprovante:', error);
  }

}
```

#### Retornar Transações Registradas para um Cartão Escaneado

```javascript
import { fetchTransactionsForCard } from 'rn-stone-lib';

async function getTransactionsForCard() {

  try {
    const transactions = await fetchTransactionsForCard();

    console.log('Transações do cartão:', transactions);
  } catch (error) {
    console.error('Erro ao obter transações do cartão:', error);
  }

}
```

#### Exibir uma Mensagem no PinPad

```javascript
import { displayMessageInPinPad } from 'rn-stone-lib';

async function showMessageOnPinPad() {

  try {
    const success = await displayMessageInPinPad('Olá, Cliente!');

    if (success) {
      console.log('Mensagem exibida no PinPad com sucesso!');
    } else {
      console.log('Falha ao exibir mensagem no PinPad.');
    }
  } catch (error) {
    console.error('Erro ao exibir mensagem no PinPad:', error);
  }

}
```

#### Conectar a um PinPad Bluetooth

```javascript

import { connectToPinPad } from  'rn-stone-lib';

async  function  connectPinPad() {

  try {
    const  success = await  connectToPinPad('PinPadName', 'MAC_ADDRESS');

    if (success) {
      console.log('Conectado ao PinPad com sucesso!');
    } else {
      console.log('Falha na conexão com o PinPad.');
    }
  } catch (error) {
    console.error('Erro:', error);
  }

}
```

#### Imprimir Recibo na Impressora POS

```javascript
import { printReceiptInPOSPrinter } from 'rn-stone-lib';

async function printReceipt(atk) {
  try {
    const success = await printReceiptInPOSPrinter('CLIENT', atk, false);

    if (success) {
      console.log('Recibo impresso com sucesso!');
    } else {
      console.log('Falha ao imprimir o recibo.');
    }
  } catch (error) {
    console.error('Erro ao imprimir recibo:', error);
  }

}
```

#### Imprimir Conteúdo HTML na Impressora POS

```javascript
import { printHTMLInPOSPrinter } from 'rn-stone-lib';

async function printCustomHTML() {

  try {
    const htmlContent = `
      <html>
        <body>
          <h1>Recibo Personalizado</h1>
          <p>Obrigado por sua compra!</p>
        </body>
      </html>
    `;

    const success = await printHTMLInPOSPrinter(htmlContent);

    if (success) {
      console.log('Conteúdo HTML impresso com sucesso!');
    } else {
      console.log('Falha ao imprimir conteúdo HTML.');
    }
  } catch (error) {
    console.error('Erro ao imprimir conteúdo HTML:', error);
  }

}
```

#### Detectar um Cartão Mifare

```javascript
import { mifareDetectCard } from 'rn-stone-lib';

async function detectMifareCard() {

  try {
    const cardUuid = await mifareDetectCard();

    console.log('Cartão Mifare detectado:', cardUuid);
  } catch (error) {
    console.error('Erro ao detectar cartão Mifare:', error);
  }

}
```

#### Autenticar um Setor Mifare

```javascript
import { mifareAuthenticateSector, MifareKeyType } from 'rn-stone-lib';

async function authenticateMifareSector() {
  try {
    const success = await mifareAuthenticateSector(
      MifareKeyType.TypeA,
      1, // Setor
      'FFFFFFFFFFFF' // Chave padrão
    );

    if (success) {
      console.log('Setor autenticado com sucesso!');
    } else {
      console.log('Falha na autenticação do setor.');
    }
  } catch (error) {
    console.error('Erro ao autenticar setor Mifare:', error);
  }

}
```

#### Ler um Bloco de um Setor Mifare

```javascript
import { mifareReadBlock, MifareKeyType } from 'rn-stone-lib';

async function readMifareBlock() {

  try {
    const data = await mifareReadBlock(
      MifareKeyType.TypeA,
      1, // Setor
      0, // Bloco
      'FFFFFFFFFFFF' // Chave padrão
    );

    console.log('Dados do bloco:', data);
  } catch (error) {
    console.error('Erro ao ler bloco Mifare:', error);
  }

}
```

#### Escrever em um Bloco de um Setor Mifare

```javascript
import { mifareWriteBlock, MifareKeyType } from 'rn-stone-lib';

async function writeMifareBlock() {

  try {
    const dataToWrite = '0123456789ABCDEF'; // Deve ter 16 bytes
    const success = await mifareWriteBlock(
      MifareKeyType.TypeA,
      1, // Setor
      0, // Bloco
      dataToWrite,
      'FFFFFFFFFFFF' // Chave padrão
    );

    if (success) {
      console.log('Dados escritos com sucesso no bloco!');
    } else {
      console.log('Falha ao escrever no bloco.');
    }
  } catch (error) {
    console.error('Erro ao escrever no bloco Mifare:', error);
  }

}
```

#### Utilizando Eventos Nativos

Você pode utilizar o hook `useNativeEventListener` para escutar eventos nativos:

```javascript
import { useNativeEventListener } from  'rn-stone-lib';

function  App() {

  useNativeEventListener('MAKE_TRANSACTION_PROGRESS', (data) => {

  console.log('Progresso:', data);

  });

  // ...

}
```

## 📝 Contribuindo

Consulte o [guia de contribuição](CONTRIBUTING.md) para saber como contribuir e entender o fluxo de desenvolvimento.

## 📄 Licença

[MIT](LICENSE)

----------

Feito com ❤️ por [seu nome ou equipe].
