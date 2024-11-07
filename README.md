import javax.swing.border.LineBorder;
import java.awt.*;
import java.awt.event.ActionListener;
import java.util.HashMap;
import java.util.Map;
import java.util.Random;

public class HotelApp extends JFrame {

    private final String[] roomStatus = new String[30];
    private final Map<Integer, String[]> roomDetails = new HashMap<>();
    private final Map<Integer, String> clientNames = new HashMap<>();
    private final Map<String, String> employeeData = new HashMap<>();
    private final Map<Integer, String[]> clientReservations = new HashMap<>();
    private final Map<Integer, String[]> employeeList = new HashMap<>();
    private final Map<Integer, String[]> guestRoomAssignments = new HashMap<>();
    private final Map<Integer, String[]> previousReservations = new HashMap<>();
    private String currentUser   = "";

    public HotelApp() {
        setTitle("Sistema Gerenciamento Hotel Ideal");
        setExtendedState(JFrame.MAXIMIZED_BOTH);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        // Inicializa status dos quartos
        for (int i = 0; i < roomStatus.length; i++) {
            roomStatus[i] = "Livre";
            roomDetails.put(i, new String[]{"", "", ""});
        }

        // Usuário padrão
        employeeData.put("Admin", "123");

        // Tela de login
        showLoginScreen();
    }

    private JPanel createPanel() {
        JPanel panel = new CustomPanel();
        panel.setBorder(new LineBorder(Color.BLACK, 1));
        return panel;
    }

    private JPanel createBorderedPanel() {
        JPanel panel = createPanel();
        panel.setBorder(BorderFactory.createLineBorder(Color.ORANGE, 5));
        return panel;
    }

    private void showLoginScreen() {
        JPanel loginPanel = createBorderedPanel();
        loginPanel.setLayout(new GridBagLayout());
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(10, 10, 10, 10);
        gbc.fill = GridBagConstraints.HORIZONTAL;

        gbc.gridx = 0;
        gbc.gridy = 0;
        gbc.gridwidth = 2;
        JLabel titleLabel = new JLabel("Sistema Gerenciamento Hotel Ideal");
        titleLabel.setFont(new Font("Arial", Font.BOLD, 24));
        loginPanel.add(titleLabel, gbc);

        gbc.gridx = 0;
        gbc.gridy = 1;
        gbc.gridwidth = 1;
        loginPanel.add(new JLabel("Usuário:"), gbc);
        JTextField userField = new JTextField(15);
        gbc.gridx = 1; gbc.gridy = 1;
        loginPanel.add(userField, gbc);

        gbc.gridx = 0; gbc.gridy = 2;
        loginPanel.add(new JLabel("Senha:"), gbc);
        JPasswordField passwordField = new JPasswordField(15);
        gbc.gridx = 1; gbc.gridy = 2;
        loginPanel.add(passwordField, gbc);

        JButton loginButton = new JButton("Login");
        JButton registerEmployeeButton = new JButton("Cadastrar Funcionário");
        JButton forgotPasswordButton = new JButton("Esqueceu a Senha?");
        gbc.gridx = 0; gbc.gridy = 3;
        loginPanel.add(loginButton, gbc);
        gbc.gridx = 1; gbc.gridy = 3;
        loginPanel.add(registerEmployeeButton, gbc);
        gbc.gridx = 0; gbc.gridy = 4; gbc.gridwidth = 2;
        loginPanel.add(forgotPasswordButton, gbc);

        this.setContentPane(loginPanel);
        this.revalidate();
        this.repaint();

        loginButton.addActionListener(e -> {
            if (userField.getText().isEmpty() || passwordField.getPassword().length == 0) {
                JOptionPane.showMessageDialog(this, "Usuário e Senha são obrigatórios!", "Erro", JOptionPane.ERROR_MESSAGE);
            } else {
                String password = new String(passwordField.getPassword());
                if (employeeData.containsKey(userField.getText()) && employeeData.get(userField.getText()).equals(password)) {
                    currentUser   = userField.getText();
                    JOptionPane.showMessageDialog(this, "Login realizado com sucesso.");
                    showMainDashboard();
                } else {
                    JOptionPane.showMessageDialog(this, "Usuário ou senha inválidos!", "Erro", JOptionPane.ERROR_MESSAGE);
                }
            }
        });

        registerEmployeeButton.addActionListener(e -> showEmployeeRegistration());
        forgotPasswordButton.addActionListener(e -> showForgotPasswordScreen ());
    }

    private void showMainDashboard() {
        JPanel dashboardPanel = createPanel();
        dashboardPanel.setLayout(new BorderLayout());

        JPanel menuPanel = new JPanel();
        menuPanel.setLayout(new FlowLayout(FlowLayout.CENTER, 10, 10));

        JButton manageCustomersButton = new JButton("Clientes");
        manageCustomersButton.setFont(new Font("Arial", Font.BOLD, 16));
        manageCustomersButton.setPreferredSize(new Dimension(150, 40));
        manageCustomersButton.setToolTipText("Gerenciar clientes");

        JButton manageRoomsButton = new JButton("Quartos");
        manageRoomsButton.setFont(new Font("Arial", Font.BOLD, 16));
        manageRoomsButton.setPreferredSize(new Dimension(150, 40));
        manageRoomsButton.setIcon(new ImageIcon("icone_quarto.png"));
        manageRoomsButton.setToolTipText("Gerenciar quartos");

        JButton manageGuestsButton = new JButton("Hóspedes");
        manageGuestsButton.setFont(new Font("Arial", Font.BOLD, 16));
        manageGuestsButton.setPreferredSize(new Dimension(150, 40));
        manageGuestsButton.setIcon(new ImageIcon("icone_hospede.png"));
        manageGuestsButton.setToolTipText("Gerenciar hóspedes");

        JButton viewClientsButton = new JButton("Reservas");
        viewClientsButton.setFont(new Font("Arial", Font.BOLD, 16));
        viewClientsButton.setPreferredSize(new Dimension(150, 40));
        viewClientsButton.setIcon(new ImageIcon("icone_reserva.png"));
        viewClientsButton.setToolTipText("Visualizar reservas");

        JButton viewEmployeesButton = new JButton("Funcionários");
        viewEmployeesButton.setFont(new Font("Arial", Font.BOLD, 16));
        viewEmployeesButton.setPreferredSize(new Dimension(150, 40));
        viewEmployeesButton.setIcon(new ImageIcon("icone_funcionario.png"));
        viewEmployeesButton.setToolTipText("Visualizar funcionários");

        JButton viewGuestsButton = new JButton("Hóspedes e Quartos");
        viewGuestsButton.setFont(new Font("Arial", Font.BOLD, 16));
        viewGuestsButton.setPreferredSize(new Dimension(200, 40));
        viewGuestsButton.setToolTipText("Visualizar hóspedes e seus quartos");

        JButton viewPreviousReservationsButton = new JButton("Reservas Anteriores");
        viewPreviousReservationsButton.setFont(new Font("Arial", Font.BOLD, 16));
        viewPreviousReservationsButton.setPreferredSize(new Dimension(200, 40));
        viewPreviousReservationsButton.setToolTipText("Visualizar reservas anteriores");

        menuPanel.add(manageCustomersButton);
        menuPanel.add(manageRoomsButton);
        menuPanel.add(manageGuestsButton);
        menuPanel.add(viewClientsButton);
        menuPanel.add(viewEmployeesButton);
        menuPanel.add(viewGuestsButton);
        menuPanel.add(viewPreviousReservationsButton);

        dashboardPanel.add(menuPanel, BorderLayout.CENTER);

        JButton logoutButton = new JButton("Sair");
        logoutButton.setFont(new Font("Arial", Font.BOLD, 16));
        logoutButton.setPreferredSize(new Dimension(150, 40));
        logoutButton.setIcon(new ImageIcon("icone_sair.png"));
        logoutButton.setToolTipText("Sair do sistema");
        dashboardPanel.add(logoutButton, BorderLayout.SOUTH);

        this.setContentPane(dashboardPanel);
        this.revalidate();

        manageCustomersButton.addActionListener(e -> showCustomerRegistration());
        manageRoomsButton.addActionListener(e -> showRoomManagement());
        manageGuestsButton.addActionListener(e -> showGuestRegistration());
        viewClientsButton.addActionListener(e -> showClientReservationsPanel());
        viewEmployeesButton.addActionListener(e -> showEmployeeList());
        viewGuestsButton.addActionListener(e -> showGuestsAndRoomsPanel());
        viewPreviousReservationsButton.addActionListener(e -> showPreviousReservationsPanel());
        logoutButton.addActionListener(e -> {
            currentUser   = "";
            showLoginScreen();
        });
    }

    private void showRoomManagement() {
        JPanel roomPanel = createPanel();
        roomPanel.setLayout(new GridLayout(0, 4));

        JButton backButton = new JButton("Voltar");
        backButton.addActionListener(e -> showMainDashboard());
        roomPanel.add(backButton);

        for (int i = 0; i < roomStatus.length; i++) {
            final int roomIndex = i;
            JButton roomButton = new JButton("Quarto " + (i + 1));

            roomButton.setBackground(getRoomColor(roomStatus[i]));
            roomButton.setBorder(new LineBorder(Color.BLACK, 1));

            roomButton.addActionListener(e -> showRoomDetails(roomIndex));
            roomPanel.add(roomButton);
        }

        this.setContentPane(roomPanel);
        this.revalidate();
    }

    private Color getRoomColor(String status) {
        return switch (status) {
            case "Livre" -> Color.GREEN;
            case "Ocupado" -> Color.RED;
            case "Manutenção" -> Color.YELLOW;
            case "Reservado para Empresa Parceira" -> Color.BLUE;
            case "Quarto de Funcionário" -> Color.MAGENTA;
            default -> Color.GRAY;
        };
    }

    private void showRoomDetails(int roomIndex) {
        String status = roomStatus[roomIndex];
        JPanel panel = new JPanel(new GridLayout(0, 2));

        JLabel statusLabel = new JLabel("Status atual:");
        JComboBox<String> statusBox = new JComboBox<>(new String[]{"Livre", "Ocupado", "Manutenção", "Reservado para Empresa Parceira", "Quarto de Funcionário"});
        statusBox.setSelectedItem(status);
        panel.add(statusLabel);
        panel.add(statusBox);

        JLabel clienteLabel = new JLabel("Cliente:");
        JComboBox<String> clienteBox = new JComboBox<>(clientNames.values().toArray(new String[0]));
        clienteBox.setSelectedItem(roomDetails.get(roomIndex)[0]);
        panel.add(clienteLabel);
        panel.add(clienteBox);

        JLabel guestsLabel = new JLabel("Hóspedes:");
        JTextField guestsField = new JTextField(10);
        panel.add(guestsLabel);
        panel.add(guestsField);

        int result = JOptionPane.showConfirmDialog(null, panel, "Detalhes do Quarto", JOptionPane.OK_CANCEL_OPTION);

        if (result == JOptionPane.OK_OPTION) {
            roomStatus[roomIndex] = (String) statusBox.getSelectedItem();
            roomDetails.put(roomIndex, new String[]{(String) clienteBox.getSelectedItem(), guestsField.getText()});
        }

        showRoomManagement();
    }

    private void showGuestRegistration() {
        JPanel guestPanel = createBorderedPanel();
        guestPanel.setLayout(new GridBagLayout());
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(10, 10, 10, 10);

        JLabel nameLabel = new JLabel("Nome do Hóspede:");
        JTextField nameField = new JTextField(15);
        JLabel rgLabel = new JLabel("RG:");
        JTextField rgField = new JTextField(15);
        JLabel cpfLabel = new JLabel("CPF:");
        JTextField cpfField = new JTextField(15);
        JLabel emailLabel = new JLabel("Email:");
        JTextField emailField = new JTextField(15);
        JLabel phoneLabel = new JLabel("Telefone (opcional):");
        JTextField phoneField = new JTextField(15);
        JLabel roomLabel = new JLabel("Quarto:");
        JComboBox<String> roomBox = new JComboBox<>(getAvailableRooms());

        gbc.gridx = 0; gbc.gridy = 0;
        guestPanel.add(nameLabel, gbc);
        gbc.gridx = 1;
        guestPanel.add(nameField, gbc);

        gbc.gridx = 0; gbc.gridy = 1;
        guestPanel.add(rgLabel, gbc);
        gbc.gridx = 1;
        guestPanel.add(rgField, gbc);

        gbc.gridx = 0; gbc.gridy = 2;
        guestPanel.add(cpfLabel, gbc);
        gbc.gridx = 1;
        guestPanel.add(cpfField, gbc);

        gbc.gridx = 0; gbc.gridy = 3;
        guestPanel.add(emailLabel, gbc);
        gbc.gridx = 1;
        guestPanel.add(emailField, gbc);

        gbc.gridx = 0; gbc.gridy = 4;
        guestPanel.add(phoneLabel, gbc);
        gbc.gridx = 1;
        guestPanel.add(phoneField, gbc);

        gbc.gridx = 0; gbc.gridy = 5;
        guestPanel.add(roomLabel, gbc);
        gbc.gridx = 1;
        guestPanel.add(roomBox, gbc);

        JButton saveButton = new JButton("Salvar");
        gbc.gridx = 0; gbc.gridy = 6; gbc.gridwidth = 2;
        guestPanel.add(saveButton, gbc);

        JButton backButton = new JButton("Voltar");
        gbc.gridy = 7;
        guestPanel.add(backButton, gbc);

        this.setContentPane(guestPanel);
        this.revalidate();
        this.repaint();

        saveButton.addActionListener(e -> {
            if (nameField.getText().isEmpty() || rgField.getText().isEmpty() || cpfField.getText().isEmpty() || emailField.getText().isEmpty()) {
                JOptionPane.showMessageDialog(this, "Nome, RG, CPF e Email são obrigatórios!", "Erro", JOptionPane.ERROR_MESSAGE);
            } else {
                String room = (String) roomBox.getSelectedItem();
                int roomIndex = Integer.parseInt(room.split(" ")[1]) - 1;
                if (guestRoomAssignments.containsKey(roomIndex)) {
                    String[] guests = guestRoomAssignments.get(roomIndex);
                    if (guests.length < 5) {
                        String[] newGuests = new String[guests.length + 1];
                        System.arraycopy(guests, 0, newGuests, 0, guests.length);
                        newGuests[guests.length] = nameField.getText();
                        guestRoomAssignments.put(roomIndex, newGuests);
                        JOptionPane.showMessageDialog(this, "Hóspede cadastrado com sucesso!");
                    } else {
                        JOptionPane.showMessageDialog(this, "Quarto já está lotado!", "Erro", JOptionPane.ERROR_MESSAGE);
                    }
                } else {
                    guestRoomAssignments.put(roomIndex, new String[]{nameField.getText()});
                    JOptionPane.showMessageDialog(this, "Hóspede cadastrado com sucesso!");
                }
                showMainDashboard();
            }
        });

        backButton.addActionListener(e -> showMainDashboard());
    }

    private String[] getAvailableRooms() {
        String[] availableRooms = new String[roomStatus.length];
        for (int i = 0; i < roomStatus.length; i++) {
            if (roomStatus[i].equals("Livre")) {
                availableRooms[i] = "Quarto " + (i + 1);
            }
        }
        return availableRooms;
    }

    private void showGuestsAndRoomsPanel() {
        JPanel guestsAndRoomsPanel = createPanel();
        guestsAndRoomsPanel.setLayout(new BorderLayout());

        JLabel title = new JLabel("Hóspedes e Quartos");
        title.setFont(new Font("Arial", Font.BOLD, 16));
        guestsAndRoomsPanel.add(title, BorderLayout.NORTH);

        // Tabela
        String[] columnNames = {"Quarto", "Hóspedes"};
        Object[][] data = new Object[guestRoomAssignments.size()][columnNames.length];
        int i = 0;
        for (Map.Entry<Integer, String[]> entry : guestRoomAssignments.entrySet()) {
            data[i][0] = "Quarto " + (entry.getKey() + 1);
            data[i][1] = String.join(", ", entry.getValue());
            i++;
        }
        JTable guestsAndRoomsTable = new JTable(data, columnNames);
        guestsAndRoomsPanel.add(new JScrollPane(guestsAndRoomsTable), BorderLayout.CENTER);

        JButton backButton = new JButton("Voltar");
        JButton removeGuestButton = new JButton("Remover Hóspede");
        guestsAndRoomsPanel.add(backButton, BorderLayout.SOUTH);
        guestsAndRoomsPanel.add(removeGuestButton, BorderLayout.EAST);

        this.setContentPane(guestsAndRoomsPanel);
        this.revalidate();

        backButton.addActionListener(e -> showMainDashboard());

        removeGuestButton.addActionListener(e -> {
            int row = guestsAndRoomsTable.getSelectedRow();
            if (row != -1) {
                int roomIndex = Integer.parseInt(guestsAndRoomsTable.getValueAt(row, 0).toString().split(" ")[1]) - 1;
                String[] guests = guestRoomAssignments.get(roomIndex);
                if (guests.length > 1) {
                    String[] newGuests = new String[guests.length - 1];
                    System.arraycopy(guests, 1, newGuests, 0, guests.length - 1);
                    guestRoomAssignments.put(roomIndex, newGuests);
                    JOptionPane.showMessageDialog(this, "Hóspede removido com sucesso!");
                } else {
                    guestRoomAssignments.remove(roomIndex);
                    JOptionPane.showMessageDialog(this, "Hóspede removido com sucesso!");
                }
                showGuestsAndRoomsPanel();
            } else {
                JOptionPane.showMessageDialog(this, "Selecione um hóspede para remover!", "Erro", JOptionPane.ERROR_MESSAGE);
            }
        });
    }

    private void showCustomerRegistration() {
        JPanel customerPanel = createBorderedPanel();
        customerPanel.setLayout(new GridBagLayout());
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(10, 10, 10, 10);

        JLabel nameLabel = new JLabel("Nome do Cliente:");
        JTextField nameField = new JTextField(15);
        JLabel cpfLabel = new JLabel("CPF:");
        JTextField cpfField = new JTextField(15);
        JLabel rgLabel = new JLabel("RG:");
        JTextField rgField = new JTextField(15);
        JLabel ageLabel = new JLabel("Idade:");
        JTextField ageField = new JTextField(15);
        JLabel dobLabel = new JLabel("Data de Nascimento:");
        JTextField dobField = new JTextField(15);
        JLabel emailLabel = new JLabel("Email:");
        JTextField emailField = new JTextField(15);
        JLabel phoneLabel = new JLabel("Telefone (opcional):");
        JTextField phoneField = new JTextField(15);
        JLabel guestsLabel = new JLabel("Número de Hóspedes:");
        JTextField guestsField = new JTextField(15);
        JLabel checkInLabel = new JLabel("Data de Check-in:");
        JTextField checkInField = new JTextField(15);
        JLabel checkOutLabel = new JLabel("Data de Check-out:");
        JTextField checkOutField = new JTextField(15);

        gbc.gridx = 0; gbc.gridy = 0;
        customerPanel.add(nameLabel, gbc);
        gbc.gridx = 1;
        customerPanel.add(nameField, gbc);

        gbc.gridx = 0; gbc.gridy = 1;
        customerPanel.add(cpfLabel, gbc);
        gbc.gridx = 1;
        customerPanel.add(cpfField, gbc);

        gbc.gridx = 0; gbc.gridy = 2;
        customerPanel.add(rgLabel, gbc);
        gbc.gridx = 1;
        customerPanel.add(rgField, gbc);

        gbc.gridx = 0; gbc.gridy = 3;
        customerPanel.add(ageLabel, gbc);
        gbc.gridx = 1;
        customerPanel.add(ageField, gbc);

        gbc.gridx = 0; gbc.gridy = 4;
        customerPanel.add(dobLabel, gbc);
        gbc.gridx = 1;
        customerPanel.add(dobField, gbc);

        gbc.gridx = 0; gbc.gridy = 5;
        customerPanel.add(emailLabel, gbc);
        gbc.gridx = 1;
        customerPanel.add(emailField, gbc);

        gbc.gridx = 0; gbc.gridy = 6;
        customerPanel.add(phoneLabel, gbc);
        gbc.gridx = 1;
        customerPanel.add(phoneField, gbc);

        gbc.gridx = 0; gbc.gridy = 7;
        customerPanel.add(guestsLabel, gbc);
        gbc.gridx = 1;
        customerPanel.add(guestsField, gbc);

        gbc.gridx = 0; gbc.gridy = 8;
        customerPanel.add(checkInLabel, gbc);
        gbc.gridx = 1;
        customerPanel.add(checkInField, gbc);

        gbc.gridx = 0; gbc.gridy = 9;
        customerPanel.add(checkOutLabel, gbc);
        gbc.gridx = 1;
        customerPanel.add(checkOutField, gbc);

        JButton saveButton = new JButton("Salvar");
        gbc.gridx = 0; gbc.gridy = 10; gbc.gridwidth = 2;
        customerPanel.add(saveButton, gbc);

        JButton backButton = new JButton("Voltar");
        gbc.gridy = 11;
        customerPanel.add(backButton, gbc);

        this.setContentPane(customerPanel);
        this.revalidate();
        this.repaint();

        saveButton.addActionListener(e -> {
            String name = nameField.getText();
            String cpf = cpfField.getText();
            String rg = rgField.getText();
            String age = ageField.getText();
            String dob = dobField.getText();
            String email = emailField.getText();
            String phone = phoneField.getText();
            String guests = guestsField.getText();
            String checkIn = checkInField.getText();
            String checkOut = checkOutField.getText();

            if (name.isEmpty() || cpf.isEmpty() || rg.isEmpty() || age.isEmpty() || dob.isEmpty()) {
                JOptionPane.showMessageDialog(this, "Nome, CPF, RG, Idade e Data de Nascimento são obrigatórios!", "Erro", JOptionPane.ERROR_MESSAGE);
            } else if (Integer.parseInt(guests) > 5) {
                JOptionPane.showMessageDialog(this, "Número de hóspedes não pode ser maior que 5.", "Erro", JOptionPane.ERROR_MESSAGE);
            } else {
                clientNames.put(clientNames.size() + 1, name);
                clientReservations.put(clientReservations.size() + 1, new String[]{guests, checkIn, checkOut});
                JOptionPane.showMessageDialog(this, "Cliente cadastrado com sucesso! Código do cliente: " + generateClientCode());
                showMainDashboard();
            }
        });

        backButton.addActionListener(e -> showMainDashboard());
    }

    private void showEmployeeRegistration() {
        JPanel employeePanel = createBorderedPanel();
        employeePanel.setLayout(new GridBagLayout());
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(10, 10, 10, 10);

        JLabel nameLabel = new JLabel("Nome:");
        JTextField nameField = new JTextField(15);
        JLabel rgLabel = new JLabel("RG:");
        JTextField rgField = new JTextField(15);
        JLabel cpfLabel = new JLabel("CPF:");
        JTextField cpfField = new JTextField(15);
        JLabel emailLabel = new JLabel("Email:");
        JTextField emailField = new JTextField(15);
        JLabel phoneLabel = new JLabel("Telefone (opcional):");
        JTextField phoneField = new JTextField(15);
        JLabel passwordLabel = new JLabel("Senha:");
        JPasswordField passwordField = new JPasswordField(15);

        gbc.gridx = 0; gbc.gridy = 0;
        employeePanel.add(nameLabel, gbc);
        gbc.gridx = 1;
        employeePanel.add(nameField, gbc);

        gbc.gridx = 0; gbc.gridy = 1;
        employeePanel.add(rgLabel, gbc);
        gbc.gridx = 1;
        employeePanel.add(rgField, gbc);

        gbc.gridx = 0; gbc.gridy = 2;
        employeePanel.add(cpfLabel, gbc);
        gbc.gridx = 1;
        employeePanel.add(cpfField, gbc);

        gbc.gridx = 0; gbc.gridy = 3;
        employeePanel.add(emailLabel, gbc);
        gbc.gridx = 1;
        employeePanel.add(emailField, gbc);

        gbc.gridx = 0; gbc.gridy = 4;
        employeePanel.add(phoneLabel, gbc);
        gbc.gridx = 1;
        employeePanel.add(phoneField, gbc);

        gbc.gridx = 0; gbc.gridy = 5;
        employeePanel.add(passwordLabel, gbc);
        gbc.gridx = 1;
        employeePanel.add(passwordField, gbc);

        JButton saveButton = new JButton("Salvar");
        gbc.gridx = 0; gbc.gridy = 6; gbc.gridwidth = 2;
        employeePanel.add(saveButton, gbc);

        JButton cancelButton = new JButton("Cancelar");
        gbc.gridy = 7;
        employeePanel.add(cancelButton, gbc);

        this.setContentPane(employeePanel);
        this.revalidate();
        this.repaint();

        saveButton.addActionListener(e -> {
            String name = nameField.getText();
            String rg = rgField.getText();
            String cpf = cpfField.getText();
            String email = emailField.getText();
            String phone = phoneField.getText();
            String password = new String(passwordField.getPassword());

            if (name.isEmpty() || rg.isEmpty() || cpf.isEmpty() || email.isEmpty() || password.isEmpty()) {
                JOptionPane.showMessageDialog(this, "Nome, RG, CPF, Email e Senha são obrigatórios!", "Erro", JOptionPane.ERROR_MESSAGE);
            } else {
                employeeData.put(name, password);
                employeeList.put(employeeList.size() + 1, new String[]{name, cpf, email});
                JOptionPane.showMessageDialog(this, "Funcionário cadastrado com sucesso! Código do funcionário: " + generateEmployeeCode());
                showMainDashboard();
            }
        });

        cancelButton.addActionListener(e -> showLoginScreen());
    }

    private String generateClientCode() {
        Random random = new Random();
        return String.format("%06d", random.nextInt(1000000));
    }

    private String generateEmployeeCode() {
        Random random = new Random();
        return String.format("%06d", random.nextInt(1000000));
    }

    private void showEmployeeList() {
        JPanel employeeListPanel = createPanel();
        employeeListPanel.setLayout(new BorderLayout());

        JLabel title = new JLabel("Lista de Funcionários");
        title.setFont(new Font("Arial", Font.BOLD, 16));
        employeeListPanel.add(title, BorderLayout.NORTH);

        // Tabela
        String[] columnNames = {"Código", "Nome", "CPF", "Email"};
        Object[][] data = new Object[employeeList.size()][columnNames.length];
        int i = 0;
        for (Map.Entry<Integer, String[]> entry : employeeList.entrySet()) {
            data[i][0] = entry.getKey();
            data[i][1] = entry.getValue()[0];
            data[i][2] = entry.getValue()[1];
            data[i][3] = entry.getValue()[2];
            i++;
        }
        JTable employeeTable = new JTable(data, columnNames);
        employeeListPanel.add(new JScrollPane(employeeTable), BorderLayout.CENTER);

        JButton backButton = new JButton("Voltar");
        employeeListPanel.add(backButton, BorderLayout.SOUTH);

        this.setContentPane(employeeListPanel);
        this.revalidate();

        backButton.addActionListener(e -> showMainDashboard());
    }

    private void showForgotPasswordScreen() {
        JPanel forgotPasswordPanel = createPanel();
        forgotPasswordPanel.setLayout(new GridBagLayout());
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(10, 10, 10, 10);

        JLabel emailLabel = new JLabel("Email:");
        JTextField emailField = new JTextField(15);

        gbc.gridx = 0; gbc.gridy = 0;
        forgotPasswordPanel.add(emailLabel, gbc);
        gbc.gridx = 1;
        forgotPasswordPanel.add(emailField, gbc);

        JButton recoverButton = new JButton("Recuperar Senha");
        gbc.gridx = 0; gbc.gridy = 1; gbc.gridwidth = 2;
        forgotPasswordPanel.add(recoverButton, gbc);

        JButton backButton = new JButton("Voltar");
        gbc.gridy = 2;
        forgotPasswordPanel.add(backButton, gbc);

        this.setContentPane(forgotPasswordPanel);
        this.revalidate();
        this.repaint();

        recoverButton.addActionListener(e -> {
            String email = emailField.getText();
            if (email.isEmpty()) {
                JOptionPane.showMessageDialog(this, "Email é obrigatório!", "Erro", JOptionPane.ERROR_MESSAGE);
            } else {
                // Email reset senha
                JOptionPane.showMessageDialog(this, "Um email foi enviado para " + email + " com instruções para redefinir a senha.");
                showLoginScreen();
            }
        });

        backButton.addActionListener(e -> showLoginScreen());
    }

    private void showClientReservationsPanel() {
        JPanel clientReservationsPanel = createPanel();
        clientReservationsPanel.setLayout(new BorderLayout());

        JLabel title = new JLabel("Painel de Clientes e Reservas");
        title.setFont(new Font("Arial", Font.BOLD, 16));
        clientReservationsPanel.add(title, BorderLayout.NORTH);

        // Tabela
        String[] columnNames = {"Código", "Nome do Cliente", "Número de Hóspedes", "Data de Check-in ", "Data de Check-out"};
        Object[][] data = new Object[clientReservations.size()][columnNames.length];
        int i = 0;
        for (Map.Entry<Integer, String[]> entry : clientReservations.entrySet()) {
            data[i][0] = entry.getKey();
            data[i][1] = clientNames.get(entry.getKey());
            data[i][2] = entry.getValue()[0];
            data[i][3] = entry.getValue()[1];
            data[i][4] = entry.getValue()[2];
            i++;
        }
        JTable reservationsTable = new JTable(data, columnNames);
        clientReservationsPanel.add(new JScrollPane(reservationsTable), BorderLayout.CENTER);

        JButton backButton = new JButton("Voltar");
        JButton removeButton = new JButton("Remover");
        clientReservationsPanel.add(backButton, BorderLayout.SOUTH);
        clientReservationsPanel.add(removeButton, BorderLayout.EAST);

        this.setContentPane(clientReservationsPanel);
        this.revalidate();

        backButton.addActionListener(e -> showMainDashboard());

        removeButton.addActionListener(e -> {
            int row = reservationsTable.getSelectedRow();
            if (row != -1) {
                int reservationId = (int) reservationsTable.getValueAt(row, 0);
                previousReservations.put(previousReservations.size() + 1, clientReservations.get(reservationId));
                clientReservations.remove(reservationId);
                clientNames.remove(reservationId);
                JOptionPane.showMessageDialog(this, "Cliente removido com sucesso!");
                showClientReservationsPanel();
            } else {
                JOptionPane.showMessageDialog(this, "Selecione um cliente para remover!", "Erro", JOptionPane.ERROR_MESSAGE);
            }
        });
    }

    private void showPreviousReservationsPanel() {
        JPanel previousReservationsPanel = createPanel();
        previousReservationsPanel.setLayout(new BorderLayout());

        JLabel title = new JLabel("Painel de Reservas Anteriores");
        title.setFont(new Font("Arial", Font.BOLD, 16));
        previousReservationsPanel.add(title, BorderLayout.NORTH);

        // Tabela
        String[] columnNames = {"Código", "Nome do Cliente", "Número de Hóspedes", "Data de Check-in ", "Data de Check-out"};
        Object[][] data = new Object[previousReservations.size()][columnNames.length];
        int i = 0;
        for (Map.Entry<Integer, String[]> entry : previousReservations.entrySet()) {
            data[i][0] = entry.getKey();
            data[i][1] = clientNames.get(entry.getKey());
            data[i][2] = entry.getValue()[0];
            data[i][3] = entry.getValue()[1];
            data[i][4] = entry.getValue()[2];
            i++;
        }
        JTable previousReservationsTable = new JTable(data, columnNames);
        previousReservationsPanel.add(new JScrollPane(previousReservationsTable), BorderLayout.CENTER);

        JButton backButton = new JButton("Voltar");
        previousReservationsPanel.add(backButton, BorderLayout.SOUTH);

        this.setContentPane(previousReservationsPanel);
        this.revalidate();

        backButton.addActionListener(e -> showMainDashboard());
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            HotelApp app = new HotelApp();
            app.setVisible(true);
        });
    }

    private void setIcon(ImageIcon imageIcon) {
        throw new UnsupportedOperationException("Not supported yet."); // Generated from nbfs://nbhost/SystemFileSystem/Templates/Classes/Code/GeneratedMethodBody
    }

    private void Dashboard() {
        throw new UnsupportedOperationException("Not supported yet."); // Generated from nbfs://nbhost/SystemFileSystem/Templates/Classes/Code/GeneratedMethodBody
    }

    private static class CustomPanel extends JPanel {
        CustomPanel() {
            setBackground(new Color(224, 255, 255)); // Background
        }
    }

    private static class customer {

        public customer() {
        }
    }

    private static class manage {

        public manage() {
        }
    }
}
