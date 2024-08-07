import javax.swing;
import java.awt;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.image.BufferedImage;
import java.io.IOException;
import javax.imageio.ImageIO;

public class DocesBorderLayout extends JFrame implements ActionListener {
    private JTextField[] quantidadeCampos;
    private JLabel[] docesLabel;
    private JLabel[] precoLabel;
    private JButton pedidoButton;
    private double[] precos = {3.50, 2.75, 1.00};

    public DocesBorderLayout() {
        setTitle("Loja de doces da Ísis (BorderLayout)");
        setSize(600, 400);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        JPanel docesPanel = new JPanel(new GridLayout(3, 1));
        quantidadeCampos = new JTextField[3];
        docesLabel = new JLabel[3];
        precoLabel = new JLabel[3];

        String[] candyImagePaths = {"biscoito.jpeg", "Coracao.jpeg", "morango.jpeg"};
        for (int i = 0; i < 3; i++) {
            JPanel panel = new JPanel(new BorderLayout());
            try {
                BufferedImage originalImage = ImageIO.read(getClass().getResource(candyImagePaths[i]));
                BufferedImage resizedImage = resizeImage(originalImage, 100, 100);
                ImageIcon candyIcon = new ImageIcon(resizedImage);
                docesLabel[i] = new JLabel(candyIcon);
                panel.add(docesLabel[i], BorderLayout.CENTER);

                precoLabel[i] = new JLabel("R$" + precos[i]);
                precoLabel[i].setHorizontalAlignment(JLabel.RIGHT);
                panel.add(precoLabel[i], BorderLayout.LINE_END);

                quantidadeCampos[i] = new JTextField(5);
                panel.add(quantidadeCampos[i], BorderLayout.SOUTH);

                docesPanel.add(panel);
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        add(docesPanel, BorderLayout.CENTER);

        pedidoButton = new JButton("Pedir");
        pedidoButton.addActionListener(this);
        add(pedidoButton, BorderLayout.SOUTH);

        setVisible(true);
    }

    private BufferedImage resizeImage(BufferedImage originalImage, int targetWidth, int targetHeight) {
        BufferedImage resizedImage = new BufferedImage(targetWidth, targetHeight, BufferedImage.TYPE_INT_ARGB);
        Graphics2D g = resizedImage.createGraphics();
        g.drawImage(originalImage, 0, 0, targetWidth, targetHeight, null);
        g.dispose();
        return resizedImage;
    }
    public void actionPerformed(ActionEvent e) {
        if (e.getSource() == pedidoButton) {
            double total = 0;
            for (int i = 0; i < 3; i++) {
                try {
                    int quantity = Integer.parseInt(quantidadeCampos[i].getText());
                    total += precos[i] * quantity;
                } catch (NumberFormatException ex) {
                    JOptionPane.showMessageDialog(this, "Por favor, insira números válidos nas quantidades.");
                    return;
                }
            }
            JOptionPane.showMessageDialog(this, "Total da compra: R$ " + String.format("%.2f", total));
        }
    }

    public static void main(String[] args) {
        new DocesBorderLayout();
    }
}
