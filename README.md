# Modelando-o-sistema-banc-rio

class Cliente:
    def __init__(self, nome, cpf):
        self.nome = nome
        self.cpf = cpf

    def exibir_info(self):
        print(f"Cliente: {self.nome}, CPF: {self.cpf}")


class ContaBancaria:
    def __init__(self, numero, cliente):
        self.numero = numero
        self.cliente = cliente
        self.saldo = 0.0

    def depositar(self, valor):
        if valor > 0:
            self.saldo += valor
            print(f"[Depósito] R${valor:.2f} depositado com sucesso na conta {self.numero}.")
        else:
            print("[Erro] Valor inválido para depósito.")

    def sacar(self, valor):
        if valor <= 0:
            print("[Erro] Valor inválido para saque.")
        elif valor > self.saldo:
            print("[Erro] Saldo insuficiente para saque.")
        else:
            self.saldo -= valor
            print(f"[Saque] R${valor:.2f} sacado com sucesso da conta {self.numero}.")

    def exibir_saldo(self):
        print(f"Saldo da Conta {self.numero}: R${self.saldo:.2f}")


class Banco:
    def __init__(self):
        self.clientes = []
        self.contas = []

    def criar_conta(self, nome, cpf, numero_conta):
        if self._buscar_conta_por_numero(numero_conta):
            print(f"[Erro] Já existe uma conta com o número {numero_conta}.")
            return

        cliente = Cliente(nome, cpf)
        conta = ContaBancaria(numero_conta, cliente)
        self.clientes.append(cliente)
        self.contas.append(conta)
        print(f"[Sucesso] Conta {numero_conta} criada para {nome}.")

    def listar_contas(self):
        if not self.contas:
            print("Nenhuma conta cadastrada.")
        for conta in self.contas:
            print("\n--- Informações da Conta ---")
            conta.cliente.exibir_info()
            conta.exibir_saldo()

    def _buscar_conta_por_numero(self, numero):
        for conta in self.contas:
            if conta.numero == numero:
                return conta
        return None

if __name__ == "__main__":
    banco = Banco()

    banco.criar_conta("João Silva", "123.456.789-00", "001")
    banco.criar_conta("Anna Livia", "987.654.321-00", "002")

    conta_joao = banco._buscar_conta_por_numero("001")
    if conta_joao:
        conta_joao.depositar(1000)
        conta_joao.sacar(200)

    banco.listar_contas()
