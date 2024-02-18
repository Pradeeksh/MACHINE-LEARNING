import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class NN {
    private List<List<E>>[] edges;

    public NN(int numLayers, int[] nodesInLayer) {
        edges = new ArrayList[numLayers - 1];
        for (int i = 0; i < numLayers - 1; i++) {
            edges[i] = new ArrayList<>();
            for (int j = 0; j < nodesInLayer[i]; j++) {
                List<E> nodeEdges = new ArrayList<>();
                for (int k = 0; k < nodesInLayer[i + 1]; k++) {
                    nodeEdges.add(new E(0.0));
                }
                edges[i].add(nodeEdges);
            }
        }
    }

    public void setEdgeWeight(int layer, int nodeFrom, int nodeTo, double weight) {
        edges[layer - 1].get(nodeFrom).get(nodeTo).setValue(weight);
    }

    public double getEdgeWeight(int layer, int nodeFrom, int nodeTo) {
        return edges[layer - 1].get(nodeFrom).get(nodeTo).getValue();
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter the number of layers: ");
        int numLayers = scanner.nextInt();
        int[] nodesInLayer = new int[numLayers];
        for (int i = 0; i < numLayers; i++) {
            System.out.print("Enter the number of nodes in layer " + i + ": ");
            nodesInLayer[i] = scanner.nextInt();
        }
        NN nn = new NN(numLayers, nodesInLayer);
        System.out.println("Enter the edge weights:");
        for (int i = 0; i < numLayers - 1; i++) {
            for (int j = 0; j < nodesInLayer[i]; j++) {
                for (int k = 0; k < nodesInLayer[i + 1]; k++) {
                    System.out.print("Enter the weight for edge from node " + j + " in layer " + (i + 1) + " to node " + k + " in layer " + (i + 2) + ": ");
                    double weight = scanner.nextDouble();
                    nn.setEdgeWeight(i + 1, j, k, weight);
                }
            }
        }
        System.out.println("Enter the node indices to query the weight (layer, nodeFrom, nodeTo):");
        int layer = scanner.nextInt();
        int nodeFrom = scanner.nextInt();
        int nodeTo = scanner.nextInt();
        double weight = nn.getEdgeWeight(layer, nodeFrom, nodeTo);
        System.out.println("Weight: " + weight);
    }
}

class E {
    private double value;

    public E(double value) {
        this.value = value;
    }

    public void setValue(double value) {
        this.value = value;
    }

    public double getValue() {
        return value;
    }
}
