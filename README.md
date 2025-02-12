<?php


$clientes = [];
$saldo = 0;


function menu() {
    global $clientes, $saldo;

    while (true) {
        readline "\n--- MENU ---\n";
        readline "1. Cadastrar Cliente\n";
        readline "2. Depositar\n";
        readline "3. Sacar\n";
        readline "4. Consultar Saldo\n";
        readline "5. Sair\n";
        readline "Escolha uma opção: ";

        $opcao = trim(fgets(STDIN));

        switch ($opcao) {
            case '1':
                cadastrarCliente();
                break;
            case '2':
                depositar();
                break;
            case '3':
                sacar();
                break;
            case '4':
                consultarSaldo();
                break;
            case '5':
                readline "Saindo...\n";
                exit;
            default:
                readline "Opção inválida! Tente novamente.\n";
        }
    }
}


function cadastrarCliente() {
    global $clientes;

    readline "Digite seu nome: ";
    $nome = trim(fgets(STDIN));

    readline "Digite seu CPF (apenas números): ";
    $cpf = trim(fgets(STDIN));

    if (validarCPF($cpf)) {
        $clientes[] = [
            'nome' => $nome,
            'cpf' => $cpf
        ];
        readline "Cliente cadastrado com sucesso!\n";
    } else {
        readline "CPF inválido! Tente novamente.\n";
    }
}


function validarCPF($cpf) {
    
    $cpf = preg_replace('/[^0-9]/', '', $cpf);

    
    if (strlen($cpf) != 11) {
        return false;
    }

    
    if (preg_match('/(\d)\1{10}/', $cpf)) {
        return false;
    }

    
    for ($t = 9; $t < 11; $t++) {
        for ($d = 0, $c = 0; $c < $t; $c++) {
            $d += $cpf[$c] * (($t + 1) - $c);
        }
        $d = ((10 * $d) % 11) % 10;
        if ($cpf[$c] != $d) {
            return false;
        }
    }

    return true;
}


function depositar() {
    global $saldo;

    readline "Digite o valor a depositar: ";
    $valor = floatval(trim(fgets(STDIN)));

    if ($valor > 0) {
        $saldo += $valor;
        readline "Depósito de R$ $valor realizado com sucesso!\n";
    } else {
        readline "Valor inválido! O depósito deve ser maior que zero.\n";
    }
}


function sacar() {
    global $saldo;

    readline "Digite o valor a sacar: ";
    $valor = floatval(trim(fgets(STDIN)));

    if ($valor > 0 && $valor <= $saldo) {
        $saldo -= $valor;
        readline "Saque de R$ $valor realizado com sucesso!\n";
    } else {
        readline "Valor inválido ou saldo insuficiente!\n";
    }
}


function consultarSaldo() {
    global $saldo;
    readline "Seu saldo atual é: R$ $saldo\n";
}


menu();
