import java.io.*;
import java.util.*;
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.List;
import java.util.ArrayList;

// Abstract base class demonstrating abstraction
abstract class Person implements Serializable {
    protected String id;
    protected String name;
    protected int age;
    protected String phone;

    public Person(String id, String name, int age, String phone) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.phone = phone;
    }

    public abstract String displayInfo();

    public String getId() {
        return id;
    }

    public String getName() {
        return name;
    }
}

// Member class demonstrating inheritance and encapsulation
class Member extends Person {
    private String membershipType;
    private Date joinDate;
    private boolean isActive;

    public Member(String id, String name, int age, String phone, String membershipType) {
        super(id, name, age, phone);
        this.membershipType = membershipType;
        this.joinDate = new Date();
        this.isActive = true;
    }

    @Override
    public String displayInfo() {
        return "=== Member Information ===\n" +
               "ID: " + id + "\n" +
               "Name: " + name + "\n" +
               "Age: " + age + "\n" +
               "Phone: " + phone + "\n" +
               "Membership Type: " + membershipType + "\n" +
               "Join Date: " + joinDate + "\n" +
               "Status: " + (isActive ? "Active" : "Inactive");
    }

    public void setActive(boolean active) {
        isActive = active;
    }

    public boolean isActive() {
        return isActive;
    }
}

// Coach class demonstrating inheritance
class Coach extends Person {
    private String specialty;
    private double salary;
    private List<String> assignedSports;

    public Coach(String id, String name, int age, String phone, String specialty, double salary) {
        super(id, name, age, phone);
        this.specialty = specialty;
        this.salary = salary;
        this.assignedSports = new ArrayList<>();
    }

    @Override
    public String displayInfo() {
        return "=== Coach Information ===\n" +
               "ID: " + id + "\n" +
               "Name: " + name + "\n" +
               "Age: " + age + "\n" +
               "Phone: " + phone + "\n" +
               "Specialty: " + specialty + "\n" +
               "Salary: $" + salary + "\n" +
               "Assigned Sports: " + (assignedSports.isEmpty() ? "None" : assignedSports);
    }

    public void assignSport(String sport) {
        assignedSports.add(sport);
    }
}

// Sports class demonstrating encapsulation
class Sport implements Serializable {
    private String sportId;
    private String sportName;
    private String category;
    private int maxParticipants;
    private List<String> enrolledMembers;

    public Sport(String sportId, String sportName, String category, int maxParticipants) {
        this.sportId = sportId;
        this.sportName = sportName;
        this.category = category;
        this.maxParticipants = maxParticipants;
        this.enrolledMembers = new ArrayList<>();
    }

    public String displayInfo() {
        return "=== Sport Information ===\n" +
               "Sport ID: " + sportId + "\n" +
               "Name: " + sportName + "\n" +
               "Category: " + category + "\n" +
               "Max Participants: " + maxParticipants + "\n" +
               "Enrolled Members: " + enrolledMembers.size() + "/" + maxParticipants;
    }

    public boolean enrollMember(String memberId) {
        if (enrolledMembers.size() < maxParticipants) {
            enrolledMembers.add(memberId);
            return true;
        }
        return false;
    }

    public String getSportId() {
        return sportId;
    }

    public String getSportName() {
        return sportName;
    }
}

// Manager class demonstrating composition and polymorphism
class ClubManager {
    private Map<String, Member> members;
    private Map<String, Coach> coaches;
    private Map<String, Sport> sports;
    private final String MEMBERS_FILE = "members.dat";
    private final String COACHES_FILE = "coaches.dat";
    private final String SPORTS_FILE = "sports.dat";

    public ClubManager() {
        members = new HashMap<>();
        coaches = new HashMap<>();
        sports = new HashMap<>();
        loadData();
    }

    public void addMember(Member member) {
        members.put(member.getId(), member);
    }

    public String displayAllMembers() {
        if (members.isEmpty()) {
            return "No members found.";
        }
        StringBuilder sb = new StringBuilder();
        for (Member member : members.values()) {
            sb.append(member.displayInfo()).append("\n\n");
        }
        return sb.toString();
    }

    public Member searchMember(String id) {
        return members.get(id);
    }

    public boolean removeMember(String id) {
        return members.remove(id) != null;
    }

    public void addCoach(Coach coach) {
        coaches.put(coach.getId(), coach);
    }

    public String displayAllCoaches() {
        if (coaches.isEmpty()) {
            return "No coaches found.";
        }
        StringBuilder sb = new StringBuilder();
        for (Coach coach : coaches.values()) {
            sb.append(coach.displayInfo()).append("\n\n");
        }
        return sb.toString();
    }

    public Coach searchCoach(String id) {
        return coaches.get(id);
    }

    public void addSport(Sport sport) {
        sports.put(sport.getSportId(), sport);
    }

    public String displayAllSports() {
        if (sports.isEmpty()) {
            return "No sports found.";
        }
        StringBuilder sb = new StringBuilder();
        for (Sport sport : sports.values()) {
            sb.append(sport.displayInfo()).append("\n\n");
        }
        return sb.toString();
    }

    public String enrollMemberInSport(String memberId, String sportId) {
        Member member = members.get(memberId);
        Sport sport = sports.get(sportId);

        if (member == null) {
            return "Member not found.";
        }
        if (sport == null) {
            return "Sport not found.";
        }

        if (sport.enrollMember(memberId)) {
            return "Member enrolled in " + sport.getSportName() + " successfully!";
        } else {
            return "Sport is full. Cannot enroll member.";
        }
    }

    public void saveData() {
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(MEMBERS_FILE))) {
            oos.writeObject(members);
        } catch (IOException e) {
            System.out.println("Error saving members: " + e.getMessage());
        }

        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(COACHES_FILE))) {
            oos.writeObject(coaches);
        } catch (IOException e) {
            System.out.println("Error saving coaches: " + e.getMessage());
        }

        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(SPORTS_FILE))) {
            oos.writeObject(sports);
        } catch (IOException e) {
            System.out.println("Error saving sports: " + e.getMessage());
        }
    }

    @SuppressWarnings("unchecked")
    private void loadData() {
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(MEMBERS_FILE))) {
            members = (Map<String, Member>) ois.readObject();
        } catch (FileNotFoundException e) {
            // File doesn't exist yet
        } catch (IOException | ClassNotFoundException e) {
            System.out.println("Error loading members: " + e.getMessage());
        }

        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(COACHES_FILE))) {
            coaches = (Map<String, Coach>) ois.readObject();
        } catch (FileNotFoundException e) {
            // File doesn't exist yet
        } catch (IOException | ClassNotFoundException e) {
            System.out.println("Error loading coaches: " + e.getMessage());
        }

        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(SPORTS_FILE))) {
            sports = (Map<String, Sport>) ois.readObject();
        } catch (FileNotFoundException e) {
            // File doesn't exist yet
        } catch (IOException | ClassNotFoundException e) {
            System.out.println("Error loading sports: " + e.getMessage());
        }
    }
}

// Main GUI class
public class SportsClubManagementSystem extends JFrame {
    private ClubManager manager;
    private JPanel mainPanel;
    private CardLayout cardLayout;

    public SportsClubManagementSystem() {
        manager = new ClubManager();
        setTitle("Sports Club Management System");
        setSize(700, 500);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        cardLayout = new CardLayout();
        mainPanel = new JPanel(cardLayout);

        createMenuPanel();
        createAddMemberPanel();
        createDisplayMembersPanel();
        createSearchMemberPanel();
        createRemoveMemberPanel();
        createAddCoachPanel();
        createDisplayCoachesPanel();
        createAddSportPanel();
        createDisplaySportsPanel();
        createEnrollMemberPanel();

        add(mainPanel);
        cardLayout.show(mainPanel, "Menu");

        // Save data on window close
        addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                manager.saveData();
            }
        });
    }

    private void createMenuPanel() {
        JPanel panel = new JPanel(new GridBagLayout());
        panel.setBackground(new Color(240, 248, 255));
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(10, 10, 10, 10);
        gbc.fill = GridBagConstraints.HORIZONTAL;

        JLabel titleLabel = new JLabel("SPORTS CLUB MANAGEMENT SYSTEM");
        titleLabel.setFont(new Font("Arial", Font.BOLD, 20));
        titleLabel.setForeground(new Color(0, 51, 102));
        gbc.gridx = 0;
        gbc.gridy = 0;
        gbc.gridwidth = 2;
        panel.add(titleLabel, gbc);

        gbc.gridwidth = 1;
        String[] buttonLabels = {
            "Add Member", "Display Members", "Search Member", "Remove Member",
            "Add Coach", "Display Coaches", "Add Sport", "Display Sports",
            "Enroll Member", "Save Data", "Exit"
        };

        for (int i = 0; i < buttonLabels.length; i++) {
            JButton btn = new JButton(buttonLabels[i]);
            btn.setPreferredSize(new Dimension(200, 40));
            btn.setBackground(new Color(70, 130, 180));
            btn.setForeground(Color.WHITE);
            btn.setFont(new Font("Arial", Font.BOLD, 12));
            btn.setFocusPainted(false);

            final int index = i;
            btn.addActionListener(e -> handleMenuAction(index));

            gbc.gridx = i % 2;
            gbc.gridy = 1 + i / 2;
            panel.add(btn, gbc);
        }

        mainPanel.add(panel, "Menu");
    }

    private void handleMenuAction(int action) {
        switch (action) {
            case 0: cardLayout.show(mainPanel, "AddMember"); break;
            case 1: cardLayout.show(mainPanel, "DisplayMembers"); break;
            case 2: cardLayout.show(mainPanel, "SearchMember"); break;
            case 3: cardLayout.show(mainPanel, "RemoveMember"); break;
            case 4: cardLayout.show(mainPanel, "AddCoach"); break;
            case 5: cardLayout.show(mainPanel, "DisplayCoaches"); break;
            case 6: cardLayout.show(mainPanel, "AddSport"); break;
            case 7: cardLayout.show(mainPanel, "DisplaySports"); break;
            case 8: cardLayout.show(mainPanel, "EnrollMember"); break;
            case 9:
                manager.saveData();
                JOptionPane.showMessageDialog(this, "Data saved successfully!");
                break;
            case 10:
                manager.saveData();
                System.exit(0);
                break;
        }
    }

    private void createAddMemberPanel() {
        JPanel panel = new JPanel(new GridBagLayout());
        panel.setBackground(new Color(240, 248, 255));
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(10, 10, 10, 10);
        gbc.anchor = GridBagConstraints.WEST;

        JLabel title = new JLabel("Add New Member");
        title.setFont(new Font("Arial", Font.BOLD, 18));
        gbc.gridx = 0;
        gbc.gridy = 0;
        gbc.gridwidth = 2;
        panel.add(title, gbc);

        gbc.gridwidth = 1;
        String[] labels = {"ID:", "Name:", "Age:", "Phone:", "Membership Type:"};
        JTextField[] fields = new JTextField[5];

        for (int i = 0; i < labels.length; i++) {
            gbc.gridx = 0;
            gbc.gridy = i + 1;
            panel.add(new JLabel(labels[i]), gbc);

            fields[i] = new JTextField(20);
            gbc.gridx = 1;
            panel.add(fields[i], gbc);
        }

        JPanel buttonPanel = new JPanel();
        JButton addBtn = new JButton("Add Member");
        JButton backBtn = new JButton("Back");

        addBtn.addActionListener(e -> {
            try {
                String id = fields[0].getText();
                String name = fields[1].getText();
                int age = Integer.parseInt(fields[2].getText());
                String phone = fields[3].getText();
                String type = fields[4].getText();

                Member member = new Member(id, name, age, phone, type);
                manager.addMember(member);
                JOptionPane.showMessageDialog(this, "Member added successfully!");

                for (JTextField field : fields) field.setText("");
            } catch (NumberFormatException ex) {
                JOptionPane.showMessageDialog(this, "Invalid age!", "Error", JOptionPane.ERROR_MESSAGE);
            }
        });

        backBtn.addActionListener(e -> cardLayout.show(mainPanel, "Menu"));

        buttonPanel.add(addBtn);
        buttonPanel.add(backBtn);
        gbc.gridx = 0;
        gbc.gridy = 6;
        gbc.gridwidth = 2;
        panel.add(buttonPanel, gbc);

        mainPanel.add(panel, "AddMember");
    }

    private void createDisplayMembersPanel() {
        JPanel panel = new JPanel(new BorderLayout());
        panel.setBackground(new Color(240, 248, 255));

        JLabel title = new JLabel("All Members", JLabel.CENTER);
        title.setFont(new Font("Arial", Font.BOLD, 18));
        panel.add(title, BorderLayout.NORTH);

        JTextArea textArea = new JTextArea();
        textArea.setEditable(false);
        textArea.setFont(new Font("Monospaced", Font.PLAIN, 12));
        JScrollPane scrollPane = new JScrollPane(textArea);

        JButton refreshBtn = new JButton("Refresh");
        JButton backBtn = new JButton("Back");

        refreshBtn.addActionListener(e -> textArea.setText(manager.displayAllMembers()));
        backBtn.addActionListener(e -> cardLayout.show(mainPanel, "Menu"));

        JPanel buttonPanel = new JPanel();
        buttonPanel.add(refreshBtn);
        buttonPanel.add(backBtn);

        panel.add(scrollPane, BorderLayout.CENTER);
        panel.add(buttonPanel, BorderLayout.SOUTH);

        mainPanel.add(panel, "DisplayMembers");
    }

    private void createSearchMemberPanel() {
        JPanel panel = new JPanel(new BorderLayout());
        panel.setBackground(new Color(240, 248, 255));

        JPanel topPanel = new JPanel();
        topPanel.add(new JLabel("Enter Member ID:"));
        JTextField idField = new JTextField(15);
        topPanel.add(idField);

        JTextArea resultArea = new JTextArea(10, 40);
        resultArea.setEditable(false);
        JScrollPane scrollPane = new JScrollPane(resultArea);

        JButton searchBtn = new JButton("Search");
        JButton backBtn = new JButton("Back");

        searchBtn.addActionListener(e -> {
            Member member = manager.searchMember(idField.getText());
            if (member != null) {
                resultArea.setText(member.displayInfo());
            } else {
                resultArea.setText("Member not found.");
            }
        });

        backBtn.addActionListener(e -> cardLayout.show(mainPanel, "Menu"));

        JPanel buttonPanel = new JPanel();
        buttonPanel.add(searchBtn);
        buttonPanel.add(backBtn);

        panel.add(topPanel, BorderLayout.NORTH);
        panel.add(scrollPane, BorderLayout.CENTER);
        panel.add(buttonPanel, BorderLayout.SOUTH);

        mainPanel.add(panel, "SearchMember");
    }

    private void createRemoveMemberPanel() {
        JPanel panel = new JPanel(new BorderLayout());
        panel.setBackground(new Color(240, 248, 255));

        JPanel topPanel = new JPanel();
        topPanel.add(new JLabel("Enter Member ID to Remove:"));
        JTextField idField = new JTextField(15);
        topPanel.add(idField);

        JButton removeBtn = new JButton("Remove");
        JButton backBtn = new JButton("Back");

        removeBtn.addActionListener(e -> {
            if (manager.removeMember(idField.getText())) {
                JOptionPane.showMessageDialog(this, "Member removed successfully!");
            } else {
                JOptionPane.showMessageDialog(this, "Member not found.", "Error", JOptionPane.ERROR_MESSAGE);
            }
            idField.setText("");
        });

        backBtn.addActionListener(e -> cardLayout.show(mainPanel, "Menu"));

        JPanel buttonPanel = new JPanel();
        buttonPanel.add(removeBtn);
        buttonPanel.add(backBtn);

        panel.add(topPanel, BorderLayout.NORTH);
        panel.add(buttonPanel, BorderLayout.SOUTH);

        mainPanel.add(panel, "RemoveMember");
    }

    private void createAddCoachPanel() {
        JPanel panel = new JPanel(new GridBagLayout());
        panel.setBackground(new Color(240, 248, 255));
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(10, 10, 10, 10);
        gbc.anchor = GridBagConstraints.WEST;

        JLabel title = new JLabel("Add New Coach");
        title.setFont(new Font("Arial", Font.BOLD, 18));
        gbc.gridx = 0;
        gbc.gridy = 0;
        gbc.gridwidth = 2;
        panel.add(title, gbc);

        gbc.gridwidth = 1;
        String[] labels = {"ID:", "Name:", "Age:", "Phone:", "Specialty:", "Salary:"};
        JTextField[] fields = new JTextField[6];

        for (int i = 0; i < labels.length; i++) {
            gbc.gridx = 0;
            gbc.gridy = i + 1;
            panel.add(new JLabel(labels[i]), gbc);

            fields[i] = new JTextField(20);
            gbc.gridx = 1;
            panel.add(fields[i], gbc);
        }

        JPanel buttonPanel = new JPanel();
        JButton addBtn = new JButton("Add Coach");
        JButton backBtn = new JButton("Back");

        addBtn.addActionListener(e -> {
            try {
                String id = fields[0].getText();
                String name = fields[1].getText();
                int age = Integer.parseInt(fields[2].getText());
                String phone = fields[3].getText();
                String specialty = fields[4].getText();
                double salary = Double.parseDouble(fields[5].getText());

                Coach coach = new Coach(id, name, age, phone, specialty, salary);
                manager.addCoach(coach);
                JOptionPane.showMessageDialog(this, "Coach added successfully!");

                for (JTextField field : fields) field.setText("");
            } catch (NumberFormatException ex) {
                JOptionPane.showMessageDialog(this, "Invalid input!", "Error", JOptionPane.ERROR_MESSAGE);
            }
        });

        backBtn.addActionListener(e -> cardLayout.show(mainPanel, "Menu"));

        buttonPanel.add(addBtn);
        buttonPanel.add(backBtn);
        gbc.gridx = 0;
        gbc.gridy = 7;
        gbc.gridwidth = 2;
        panel.add(buttonPanel, gbc);

        mainPanel.add(panel, "AddCoach");
    }

    private void createDisplayCoachesPanel() {
        JPanel panel = new JPanel(new BorderLayout());
        panel.setBackground(new Color(240, 248, 255));

        JLabel title = new JLabel("All Coaches", JLabel.CENTER);
        title.setFont(new Font("Arial", Font.BOLD, 18));
        panel.add(title, BorderLayout.NORTH);

        JTextArea textArea = new JTextArea();
        textArea.setEditable(false);
        textArea.setFont(new Font("Monospaced", Font.PLAIN, 12));
        JScrollPane scrollPane = new JScrollPane(textArea);

        JButton refreshBtn = new JButton("Refresh");
        JButton backBtn = new JButton("Back");

        refreshBtn.addActionListener(e -> textArea.setText(manager.displayAllCoaches()));
        backBtn.addActionListener(e -> cardLayout.show(mainPanel, "Menu"));

        JPanel buttonPanel = new JPanel();
        buttonPanel.add(refreshBtn);
        buttonPanel.add(backBtn);

        panel.add(scrollPane, BorderLayout.CENTER);
        panel.add(buttonPanel, BorderLayout.SOUTH);

        mainPanel.add(panel, "DisplayCoaches");
    }

    private void createAddSportPanel() {
        JPanel panel = new JPanel(new GridBagLayout());
        panel.setBackground(new Color(240, 248, 255));
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(10, 10, 10, 10);
        gbc.anchor = GridBagConstraints.WEST;

        JLabel title = new JLabel("Add New Sport");
        title.setFont(new Font("Arial", Font.BOLD, 18));
        gbc.gridx = 0;
        gbc.gridy = 0;
        gbc.gridwidth = 2;
        panel.add(title, gbc);

        gbc.gridwidth = 1;
        String[] labels = {"Sport ID:", "Sport Name:", "Category:", "Max Participants:"};
        JTextField[] fields = new JTextField[4];

        for (int i = 0; i < labels.length; i++) {
            gbc.gridx = 0;
            gbc.gridy = i + 1;
            panel.add(new JLabel(labels[i]), gbc);

            fields[i] = new JTextField(20);
            gbc.gridx = 1;
            panel.add(fields[i], gbc);
        }

        JPanel buttonPanel = new JPanel();
        JButton addBtn = new JButton("Add Sport");
        JButton backBtn = new JButton("Back");

        addBtn.addActionListener(e -> {
            try {
                String sportId = fields[0].getText();
                String sportName = fields[1].getText();
                String category = fields[2].getText();
                int maxParticipants = Integer.parseInt(fields[3].getText());

                Sport sport = new Sport(sportId, sportName, category, maxParticipants);
                manager.addSport(sport);
                JOptionPane.showMessageDialog(this, "Sport added successfully!");

                for (JTextField field : fields) field.setText("");
            } catch (NumberFormatException ex) {
                JOptionPane.showMessageDialog(this, "Invalid input!", "Error", JOptionPane.ERROR_MESSAGE);
            }
        });

        backBtn.addActionListener(e -> cardLayout.show(mainPanel, "Menu"));

        buttonPanel.add(addBtn);
        buttonPanel.add(backBtn);
        gbc.gridx = 0;
        gbc.gridy = 5;
        gbc.gridwidth = 2;
        panel.add(buttonPanel, gbc);

        mainPanel.add(panel, "AddSport");
    }

    private void createDisplaySportsPanel() {
        JPanel panel = new JPanel(new BorderLayout());
        panel.setBackground(new Color(240, 248, 255));

        JLabel title = new JLabel("All Sports", JLabel.CENTER);
        title.setFont(new Font("Arial", Font.BOLD, 18));
        panel.add(title, BorderLayout.NORTH);

        JTextArea textArea = new JTextArea();
        textArea.setEditable(false);
        textArea.setFont(new Font("Monospaced", Font.PLAIN, 12));
        JScrollPane scrollPane = new JScrollPane(textArea);

        JButton refreshBtn = new JButton("Refresh");
        JButton backBtn = new JButton("Back");

        refreshBtn.addActionListener(e -> textArea.setText(manager.displayAllSports()));
        backBtn.addActionListener(e -> cardLayout.show(mainPanel, "Menu"));

        JPanel buttonPanel = new JPanel();
        buttonPanel.add(refreshBtn);
        buttonPanel.add(backBtn);

        panel.add(scrollPane, BorderLayout.CENTER);
        panel.add(buttonPanel, BorderLayout.SOUTH);

        mainPanel.add(panel, "DisplaySports");
    }

    private void createEnrollMemberPanel() {
        JPanel panel = new JPanel(new GridBagLayout());
        panel.setBackground(new Color(240, 248, 255));
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(10, 10, 10, 10);
        gbc.anchor = GridBagConstraints.WEST;

        JLabel title = new JLabel("Enroll Member in Sport");
        title.setFont(new Font("Arial", Font.BOLD, 18));
        gbc.gridx = 0;
        gbc.gridy = 0;
        gbc.gridwidth = 2;
        panel.add(title, gbc);

        gbc.gridwidth = 1;
        gbc.gridx = 0;
        gbc.gridy = 1;
        panel.add(new JLabel("Member ID:"), gbc);

        JTextField memberIdField = new JTextField(20);
        gbc.gridx = 1;
        panel.add(memberIdField, gbc);

        gbc.gridx = 0;
        gbc.gridy = 2;
        panel.add(new JLabel("Sport ID:"), gbc);

        JTextField sportIdField = new JTextField(20);
        gbc.gridx = 1;
        panel.add(sportIdField, gbc);

        JPanel buttonPanel = new JPanel();
        JButton enrollBtn = new JButton("Enroll");
        JButton backBtn = new JButton("Back");

        enrollBtn.addActionListener(e -> {
            String result = manager.enrollMemberInSport(memberIdField.getText(), sportIdField.getText());
            JOptionPane.showMessageDialog(this, result);
            memberIdField.setText("");
            sportIdField.setText("");
        });

        backBtn.addActionListener(e -> cardLayout.show(mainPanel, "Menu"));

        buttonPanel.add(enrollBtn);
        buttonPanel.add(backBtn);
        gbc.gridx = 0;
        gbc.gridy = 3;
        gbc.gridwidth = 2;
        panel.add(buttonPanel, gbc);

        mainPanel.add(panel, "EnrollMember");
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            SportsClubManagementSystem frame = new SportsClubManagementSystem();
            frame.setVisible(true);
        });
    }
}
