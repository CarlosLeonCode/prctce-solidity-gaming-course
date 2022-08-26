# Notes

<hr>

## Chapter One

# Variables y Números Enteros

Las variables se guardan de manera permanente en el contarato, es como si escribieramos
en la base de datos. Estos agregan gas al contrato.

## Tipos:

- **UINT**: Enteros positivos.

> Por convencion es recomendable llamar a los parametros de las funciones con un '\_' | De igual forma es recomendable llamar las funciones privadas con el '\_' al inicio del nombre.

<hr>

## Chapter Two

### Direcciones

La blockchaing de Ethereum se compone de **cuentas** u **Smart Contracts** que pueden ser el simil a las cuentas bancarias. Cada cuenta tiene un balance en Ether y se pueden recibir y transferir Ether entre cuentas. Cada cuenta tiene una **Dirección** que seria como el número de cuenta en un Banco.

### Mapings

Son otra manera de ordenar los datos en Solidity.

```sol
// Para una aplicación financiera, guardamos un uint con el balance de su cuenta:
mapping (address => uint) public accountBalance;
// O podría usarse para guardar / ver los usuarios basados en un userId
mapping (uint => string) userIdToName;
```

Un mapeo es esencialmente una asociación valor-clave para guardar y ver datos. En el primer ejemplo, la clave es un address (dirección) y el valor correspondería a un uint, y en el segundo ejemplo la clave es un uint y el valor un string.

### msg.sender

Variable global que posee la dirección **address** del contrato.

### require.require

```sol
function sayHiToVitalik(string _name) public returns (string) {
  // Compara si _name es igual a "Vitalik". Lanza un error si no lo son.
  // (Nota: Solidity no tiene su propio comparador de strings, por lo que
  // compararemos sus hashes keccak256 para ver si sus strings son iguales)
  require(keccak256(_name) == keccak256("Vitalik"));
  // Si es verdad, continuamos con la función:
  return "Hi!";
}
```

Si llamas a la función con sayHiToVitalik("Vitalik"), esta devolverá "Hi!". Si la llamas con cualquier otra entrada, lanzará un error y no se ejecutará.

De este modo require es muy útil a la hora de verificar que ciertas condiciones sean verdaderas antes de ejecutar una función.

### Herencia

```sol
contract Doge {
  function catchphrase() public returns (string) {
    return "So Wow CryptoDoge";
  }
}

contract BabyDoge is Doge {
  function anotherCatchphrase() public returns (string) {
    return "Such Moon BabyDoge";
  }
}
```
BabyDoge hereda de Doge. Eso significa que si compilas y ejecutas BabyDoge, este tendrá acceso tanto a catchphrase() como a anotherCatchphrase() (y a cualquier otra función publica que definamos en Doge).

Esto puede usarse como una herencia lógica (como una subclase, un Gato es un Animal). Pero también puede usarse simplemente para organizar tu código agrupando lógica similar en diferentes clases.

### Import

Nos permite importar otros contratos dentro de uno.

```sol
import "./someothercontract.sol";

contract newContract is SomeOtherContract {
  // Code 
}
```

### Storage vs Memory

En Solidity, hay dos sitios donde puedes guardar variables - en storage y en memory.

Storage se refiere a las variables guardadas permanentemente en la blockchain. Las variables de tipo memory son temporales, y son borradas en lo que una función externa llama a tu contrato. Piensa que es como el disco duro vs la RAM de tu ordenador.

La mayoría del tiempo no necesitas usar estas palabras clave ya que Solidity las controla por defecto. Las variables de estado (variables declaradas fuera de las funciones) son por defecto del tipo storage y son guardadas permanentemente en la blockchain, mientras que las variables declaradas dentro de las funciones son por defecto del tipo memory y desaparecerán una vez la llamada a la función haya terminado.

Aun así, hay momentos en los que necesitas usar estas palabras clave, concretamente cuando estes trabajando con structs y arrays dentro de las funciones:

```sol
// De modo que deberias declararlo como `storage`, así:
Sandwich storage mySandwich = sandwiches[_index];
// ...donde `mySandwich` es un apuntador a `sandwiches[_index]`
// de tipo storage, y...
```
