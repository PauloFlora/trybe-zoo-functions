## O que é o projeto Trybe Zoo Functions

O objetivo deste projeto é treinar a lógica de programação, produzir código legível, conciso e expressivo utilizando as novas funcionalidades do ES6;
  - Utilizar as _Higher Order Functions_ para manipular e criar arrays;
  - Escolher a _Higher Order Function_ mais adequada para a obtenção de um resultado esperado;
  - Aprender a usar de forma conjunta as _Higher Order Functions_;

Para isto, tive que implementar várias funções para atender aos requisitos propostos. 

## Exemplo: 

### Implemente a função `getEmployeesCoverage`

Esta função será responsável por associar informações de cobertura das pessoas funcionárias.

A função deve receber um objeto de opções que determinará seu comportamento, sendo:

* **name**: O nome ou sobrenome da pessoa a ser buscada
* **id**: O id da pessoa a ser buscada

Ao ser chamada sem argumentos, deverá retornar um array com a cobertura de todas as pessoas funcionárias:

Caso nenhuma pessoa seja encontrada com o nome, sobrenome ou id, deverá ser lançado um erro gerado com a função construtora **Error** da biblioteca padrão do JavaScript com a mensagem **"Informações inválidas"**. Exemplo:

``` javascript 
// getEmployeesCoverage.js
const data = require('../data/zoo_data');

const { species, employees } = data;

function getAnimals(employee) {
  const animalIds = employee.responsibleFor;
  const animals = [];
  animalIds.forEach((id) => animals.push(species.find((animal) => animal.id === id).name));
  return animals;
}

function getLocations(animals) {
  return animals.map((animal) => species.find((specie) => specie.name === animal).location);
}

function getReport(employee) {
  return employee.map((e) => {
    const result = { id: e.id };
    result.fullName = `${e.firstName} ${e.lastName}`;
    result.species = getAnimals(e);
    result.locations = getLocations(result.species);
    return result;
  });
}

function findEmployee(info) {
  const emp = employees.filter((e) => e.firstName === info || e.lastName === info || e.id === info);
  return getReport(emp)[0];
}

function checkName(name) {
  if (employees.some((employee) => employee.firstName === name || employee.lastName === name)) {
    return findEmployee(name);
  } throw new Error('Informações inválidas');
}

function checkId(id) {
  if (employees.some((employee) => employee.id === id)) {
    return findEmployee(id);
  } throw new Error('Informações inválidas');
}

function handleInfo(empInfo) {
  if (empInfo.name) {
    return checkName(empInfo.name);
  } if (empInfo.id) {
    return checkId(empInfo.id);
  }
}

function getEmployeesCoverage(employeeInfo) {
  if (employeeInfo) {
    return handleInfo(employeeInfo);
  }
  if (!employeeInfo) {
    return employees.map((employee) => handleInfo({ id: employee.id }));
  }
}

module.exports = getEmployeesCoverage;

```