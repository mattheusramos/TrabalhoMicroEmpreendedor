import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

class Produto {
    private String nome;
    private int quantidade;
    private double preco;

    public Produto(String nome, int quantidade, double preco) {
        this.nome = nome;
        this.quantidade = quantidade;
        this.preco = preco;
    }

    public String getNome() {
        return nome;
    }

    public int getQuantidade() {
        return quantidade;
    }

    public double getPreco() {
        return preco;
    }

    public void vender(int quantidade) {
        if (this.quantidade >= quantidade) {
            this.quantidade -= quantidade;
        } else {
            System.out.println("Estoque insuficiente para vender " + quantidade + " unidades de " + nome + ".");
        }
    }

    @Override
    public String toString() {
        return "Produto: " + nome + ", Quantidade: " + quantidade + ", Preço: R$" + preco;
    }
}

class Despesa {
    private String descricao;
    private double valor;

    public Despesa(String descricao, double valor) {
        this.descricao = descricao;
        this.valor = valor;
    }

    public double getValor() {
        return valor;
    }

    @Override
    public String toString() {
        return descricao + ", R$" + valor;
    }
}

class Venda {
    private String nomeProduto;
    private int quantidade;
    private double total;

    public Venda(String nomeProduto, int quantidade, double total) {
        this.nomeProduto = nomeProduto;
        this.quantidade = quantidade;
        this.total = total;
    }

    @Override
    public String toString() {
        return "Venda de " + quantidade + " unidade(s) de " + nomeProduto + ", Total: R$" + total;
    }
}

public class AppMicroEmpreendedor {
    private List<Produto> produtos;
    private List<Despesa> despesas;
    private List<Venda> vendas;

    public AppMicroEmpreendedor() {
        this.produtos = new ArrayList<>();
        this.despesas = new ArrayList<>();
        this.vendas = new ArrayList<>();
    }

    public void adicionarProduto(String nome, int quantidade, double preco) {
        Produto produto = new Produto(nome, quantidade, preco);
        produtos.add(produto);
        System.out.println("Produto adicionado: " + produto);
    }

    public void listarProdutos() {
        System.out.println("\nLista de Produtos:");
        for (Produto produto : produtos) {
            System.out.println(produto);
        }
    }

    public double calcularTotalProdutos() {
        double totalProdutos = 0.0;
        for (Produto produto : produtos) {
            totalProdutos += produto.getQuantidade() * produto.getPreco();
        }
        return totalProdutos;
    }

    public void adicionarDespesa(String descricao, double valor) {
        Despesa despesa = new Despesa(descricao, valor);
        despesas.add(despesa);
        System.out.println("Despesa adicionada: " + despesa);
    }

    public void listarDespesas() {
        System.out.println("\nLista de Despesas:");
        for (Despesa despesa : despesas) {
            System.out.println(despesa);
        }

        double totalProdutos = calcularTotalProdutos();
        double totalDespesas = 0.0;

        for (Despesa despesa : despesas) {
            totalDespesas += despesa.getValor();
        }

        double saldo = totalProdutos - totalDespesas;
        System.out.println("Saldo atual: R$" + saldo);
    }

    public void registrarVenda(String nomeProduto, int quantidade) {
        for (Produto produto : produtos) {
            if (produto.getNome().equalsIgnoreCase(nomeProduto)) {
                double totalVenda = quantidade * produto.getPreco();
                produto.vender(quantidade);
                Venda venda = new Venda(nomeProduto, quantidade, totalVenda);
                vendas.add(venda);
                System.out.println("Venda registrada: " + venda);
                return;
            }
        }
        System.out.println("Produto não encontrado: " + nomeProduto);
    }

    public void listarVendas() {
        System.out.println("\nHistórico de Vendas:");
        for (Venda venda : vendas) {
            System.out.println(venda);
        }
    }

    public double calcularLucro() {
        double totalProdutos = calcularTotalProdutos();
        double totalDespesas = 0.0;
        for (Despesa despesa : despesas) {
            totalDespesas += despesa.getValor();
        }
        double totalVendas = 0.0;
        for (Venda venda : vendas) {
            totalVendas += venda.toString().contains("Total: R$") ? Double.parseDouble(venda.toString().split("Total: R$")[1]) : 0;
        }
        return totalVendas - totalDespesas;
    }

    public static void main(String[] args) {
        AppMicroEmpreendedor app = new AppMicroEmpreendedor();
        Scanner scanner = new Scanner(System.in);
        int opcao;

        do {
            System.out.println("\nMenu:");
            System.out.println("1. Adicionar Produto");
            System.out.println("2. Listar Produtos");
            System.out.println("3. Adicionar Despesa");
            System.out.println("4. Listar Despesas");
            System.out.println("5. Registrar Venda");
            System.out.println("6. Listar Vendas");
            System.out.println("7. Calcular Lucro");
            System.out.println("0. Sair");
            System.out.print("Escolha uma opção: ");
            opcao = scanner.nextInt();
            scanner.nextLine(); // Consumir a nova linha

            switch (opcao) {
                case 1:
                    System.out.print("Informe o nome do produto: ");
                    String nomeProduto = scanner.nextLine();
                    System.out.print("Informe a quantidade: ");
                    int quantidade = scanner.nextInt();
                    System.out.print("Informe o preço: R$");
                    double preco = scanner.nextDouble();
                    app.adicionarProduto(nomeProduto, quantidade, preco);
                    break;
                case 2:
                    app.listarProdutos();
                    break;
                case 3:
                    System.out.print("Informe a descrição da despesa: ");
                    String descricaoDespesa = scanner.nextLine();
                    System.out.print("Informe o valor da despesa: R$");
                    double valorDespesa = scanner.nextDouble();
                    app.adicionarDespesa(descricaoDespesa, valorDespesa);
                    break;
                case 4:
                    app.listarDespesas();
                    break;
                case 5:
                    System.out.print("Informe o nome do produto a vender: ");
                    String nomeVenda = scanner.nextLine();
                    System.out.print("Informe a quantidade a vender: ");
                    int quantidadeVenda = scanner.nextInt();
                    app.registrarVenda(nomeVenda, quantidadeVenda);
                    break;
                case 6:
                    app.listarVendas();
                    break;
                case 7:
                    double lucro = app.calcularLucro();
                    System.out.println("Lucro total: R$" + lucro);
                    break;
                case 0:
                    System.out.println("Saindo...");
                    break;
                default:
                    System.out.println("Opção inválida!");
            }
        } while (opcao != 0);

        scanner.close();
    }
}
