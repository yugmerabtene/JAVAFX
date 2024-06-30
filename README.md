### Jour 1 : Configuration et Introduction à JavaFX

#### Matinée

**1. Configuration de l'environnement de développement**

**1.1. Installation de JDK**
- **Téléchargez** et installez le JDK depuis le site officiel d'Oracle ou utilisez OpenJDK.

**1.2. Installation d'IntelliJ IDEA**
- **Téléchargez** et installez IntelliJ IDEA depuis le site officiel de JetBrains.

**1.3. Configuration de JavaFX dans IntelliJ IDEA**
- **Création d'un nouveau projet JavaFX :**
  1. Ouvrez IntelliJ IDEA.
  2. Cliquez sur "New Project".
  3. Sélectionnez "Java" et cliquez sur "Next".
  4. Donnez un nom à votre projet et configurez le JDK.
  5. Cliquez sur "Next" puis "Finish".
  
- **Ajout des librairies JavaFX :**
  1. Téléchargez JavaFX SDK depuis le site officiel de Gluon.
  2. Extrayez le contenu du fichier ZIP téléchargé.
  3. Dans IntelliJ, faites un clic droit sur votre projet -> "Open Module Settings" (ou appuyez sur F4).
  4. Dans la section "Modules", sélectionnez votre module et allez à l'onglet "Dependencies".
  5. Cliquez sur le bouton "+" et sélectionnez "JARs or directories".
  6. Naviguez jusqu'au dossier lib de JavaFX SDK extrait et ajoutez tous les fichiers JAR.
  7. Allez dans l'onglet "Libraries" et ajoutez également les JARs de JavaFX.

**1.4. Configuration des arguments de VM**
- Pour exécuter des applications JavaFX, ajoutez les arguments de VM dans IntelliJ :
  1. Allez dans "Run" -> "Edit Configurations".
  2. Ajoutez les arguments de VM suivants dans le champ "VM options" (adaptez le chemin vers votre installation de JavaFX) :
     ```
     --module-path /path/to/javafx-sdk-17/lib --add-modules javafx.controls,javafx.fxml
     ```

**1.5. Structure d'une application JavaFX**
- Une application JavaFX de base se compose d'une classe qui hérite de `Application` et redéfinit la méthode `start`.

**Exemple minimal d'une application JavaFX :**

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Label;
import javafx.stage.Stage;

public class HelloJavaFX extends Application {
    @Override
    public void start(Stage stage) {
        Label label = new Label("Hello JavaFX!");
        Scene scene = new Scene(label, 400, 200);
        stage.setScene(scene);
        stage.setTitle("Hello JavaFX");
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

**Exercice 1 : Hello JavaFX**
- Créez une application JavaFX simple affichant "Hello JavaFX!" dans une fenêtre.

#### Après-midi

**2. Composants de base**
- **Label :** Utilisé pour afficher du texte.
- **Button :** Bouton cliquable.
- **TextField :** Champ de texte pour saisir une seule ligne de texte.
- **TextArea :** Champ de texte pour saisir plusieurs lignes de texte.
- **ComboBox :** Liste déroulante permettant de choisir parmi plusieurs options.
- **CheckBox :** Case à cocher pour activer/désactiver une option.
- **RadioButton :** Bouton radio permettant de choisir une option parmi plusieurs.

**2.1. Gestion des événements**
- Les événements sont gérés via des écouteurs (`EventHandler`). Par exemple, pour réagir à un clic de bouton, on utilise `setOnAction`.

**Exemple de formulaire simple :**

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class SimpleForm extends Application {
    @Override
    public void start(Stage stage) {
        TextField nameField = new TextField();
        TextField emailField = new TextField();
        Button submitButton = new Button("Submit");
        
        submitButton.setOnAction(e -> {
            String name = nameField.getText();
            String email = emailField.getText();
            Alert alert = new Alert(Alert.AlertType.INFORMATION, "Name: " + name + "\nEmail: " + email);
            alert.show();
        });

        VBox vbox = new VBox(10, new Label("Name:"), nameField, new Label("Email:"), emailField, submitButton);
        Scene scene = new Scene(vbox, 300, 200);
        stage.setScene(scene);
        stage.setTitle("Simple Form");
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

**Exercice 2 : Formulaire simple**
- Créez un formulaire de saisie avec des champs de texte et un bouton de soumission. Lorsque le bouton est cliqué, affichez les données saisies dans une alerte.

**3. Mise en page avec JavaFX**
- **HBox :** Disposition horizontale des composants.
- **VBox :** Disposition verticale des composants.
- **GridPane :** Disposition en grille avec lignes et colonnes.
- **BorderPane :** Disposition des composants en haut, bas, gauche, droite et centre.
- **StackPane :** Superposition des composants.

**3.1. Exemple d'interface utilisateur avec plusieurs Layouts :**

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.HBox;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class MultiLayout extends Application {
    @Override
    public void start(Stage stage) {
        BorderPane borderPane = new BorderPane();
        
        HBox topBox = new HBox(10, new Button("Button 1"), new Button("Button 2"));
        VBox leftBox = new VBox(10, new Button("Button 3"), new Button("Button 4"));
        
        borderPane.setTop(topBox);
        borderPane.setLeft(leftBox);
        borderPane.setCenter(new Button("Center Button"));
        
        Scene scene = new Scene(borderPane, 400, 300);
        stage.setScene(scene);
        stage.setTitle("Multi Layout");
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

**Exercice 3 : Interface utilisateur avec plusieurs Layouts**
- Créez une interface utilisateur utilisant différents Layouts pour organiser les composants.

### Jour 2 : Projet de gestion d'étudiants

#### Matinée

**1. Conception de l'interface de gestion des étudiants**
- **TableView :** Utilisé pour afficher les données des étudiants.
- **Button :** Boutons pour ajouter, modifier et supprimer des étudiants.
- **Dialog :** Fenêtres de dialogue pour la saisie des informations des étudiants.

**1.1. Classe Student**

```java
import javafx.beans.property.SimpleStringProperty;
import javafx.beans.property.StringProperty;

public class Student {
    private final StringProperty name;
    private final StringProperty email;

    public Student(String name, String email) {
        this.name = new SimpleStringProperty(name);
        this.email = new SimpleStringProperty(email);
    }

    public String getName() {
        return name.get();
    }

    public void setName(String name) {
        this.name.set(name);
    }

    public StringProperty nameProperty() {
        return name;
    }

    public String getEmail() {
        return email.get();
    }

    public void setEmail(String email) {
        this.email.set(email);
    }

    public StringProperty emailProperty() {
        return email;
    }
}
```

**1.2. Exemple d'interface de gestion d'étudiants**

```java
import javafx.application.Application;
import javafx.collections.FXCollections;
import javafx.collections.ObservableList;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.HBox;
import javafx.stage.Stage;

public class StudentManagement extends Application {

    private TableView<Student> table;
    private ObservableList<Student> studentList;

    @Override
    public void start(Stage stage) {
        studentList = FXCollections.observableArrayList();
        table = new TableView<>(studentList);

        TableColumn<Student, String> nameColumn = new TableColumn<>("Name");
        nameColumn.setCellValueFactory(data -> data.getValue().nameProperty());

        TableColumn<Student, String> emailColumn = new TableColumn<>("Email");
        emailColumn.setCellValueFactory(data -> data.getValue().emailProperty());

        table.getColumns().addAll(nameColumn, emailColumn);

        Button addButton = new Button("Add");
        addButton.setOnAction(e -> showAddStudentDialog());

        HBox buttonBox = new HBox(10, addButton);

        BorderPane root = new BorderPane();
        root.setCenter(table);
        root.setBottom(buttonBox);

        Scene scene = new Scene(root, 600, 400);
        stage.setScene(scene);
        stage.setTitle("Student Management");
        stage.show();
    }

    private void showAddStudentDialog() {
        Dialog<Student> dialog = new Dialog<>();
        dialog.setTitle("Add Student");

        Label nameLabel = new Label("Name:");
        TextField nameField = new TextField();
        Label emailLabel = new Label("Email:");
        TextField emailField = new TextField();



        VBox vbox = new VBox(10, nameLabel, nameField, emailLabel, emailField);
        dialog.getDialogPane().setContent(vbox);

        ButtonType addButtonType = new ButtonType("Add", ButtonBar.ButtonData.OK_DONE);
        dialog.getDialogPane().getButtonTypes().addAll(addButtonType, ButtonType.CANCEL);

        dialog.setResultConverter(buttonType -> {
            if (buttonType == addButtonType) {
                return new Student(nameField.getText(), emailField.getText());
            }
            return null;
        });

        dialog.showAndWait().ifPresent(student -> studentList.add(student));
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

#### Après-midi

**2. Finalisation du projet**
- Ajout des fonctionnalités de modification et suppression d'étudiants.
- Optimisation de l'interface utilisateur.
- Test et validation.

**2.1. Ajout de la modification et suppression**

```java
import javafx.application.Application;
import javafx.collections.FXCollections;
import javafx.collections.ObservableList;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.HBox;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class StudentManagement extends Application {

    private TableView<Student> table;
    private ObservableList<Student> studentList;

    @Override
    public void start(Stage stage) {
        studentList = FXCollections.observableArrayList();
        table = new TableView<>(studentList);

        TableColumn<Student, String> nameColumn = new TableColumn<>("Name");
        nameColumn.setCellValueFactory(data -> data.getValue().nameProperty());

        TableColumn<Student, String> emailColumn = new TableColumn<>("Email");
        emailColumn.setCellValueFactory(data -> data.getValue().emailProperty());

        table.getColumns().addAll(nameColumn, emailColumn);

        Button addButton = new Button("Add");
        addButton.setOnAction(e -> showAddStudentDialog());

        Button editButton = new Button("Edit");
        editButton.setOnAction(e -> showEditStudentDialog());

        Button deleteButton = new Button("Delete");
        deleteButton.setOnAction(e -> {
            Student selectedStudent = table.getSelectionModel().getSelectedItem();
            if (selectedStudent != null) {
                studentList.remove(selectedStudent);
            }
        });

        HBox buttonBox = new HBox(10, addButton, editButton, deleteButton);

        BorderPane root = new BorderPane();
        root.setCenter(table);
        root.setBottom(buttonBox);

        Scene scene = new Scene(root, 600, 400);
        stage.setScene(scene);
        stage.setTitle("Student Management");
        stage.show();
    }

    private void showAddStudentDialog() {
        Dialog<Student> dialog = new Dialog<>();
        dialog.setTitle("Add Student");

        Label nameLabel = new Label("Name:");
        TextField nameField = new TextField();
        Label emailLabel = new Label("Email:");
        TextField emailField = new TextField();

        VBox vbox = new VBox(10, nameLabel, nameField, emailLabel, emailField);
        dialog.getDialogPane().setContent(vbox);

        ButtonType addButtonType = new ButtonType("Add", ButtonBar.ButtonData.OK_DONE);
        dialog.getDialogPane().getButtonTypes().addAll(addButtonType, ButtonType.CANCEL);

        dialog.setResultConverter(buttonType -> {
            if (buttonType == addButtonType) {
                return new Student(nameField.getText(), emailField.getText());
            }
            return null;
        });

        dialog.showAndWait().ifPresent(student -> studentList.add(student));
    }

    private void showEditStudentDialog() {
        Student selectedStudent = table.getSelectionModel().getSelectedItem();
        if (selectedStudent == null) {
            return;
        }

        Dialog<Student> dialog = new Dialog<>();
        dialog.setTitle("Edit Student");

        Label nameLabel = new Label("Name:");
        TextField nameField = new TextField(selectedStudent.getName());
        Label emailLabel = new Label("Email:");
        TextField emailField = new TextField(selectedStudent.getEmail());

        VBox vbox = new VBox(10, nameLabel, nameField, emailLabel, emailField);
        dialog.getDialogPane().setContent(vbox);

        ButtonType saveButtonType = new ButtonType("Save", ButtonBar.ButtonData.OK_DONE);
        dialog.getDialogPane().getButtonTypes().addAll(saveButtonType, ButtonType.CANCEL);

        dialog.setResultConverter(buttonType -> {
            if (buttonType == saveButtonType) {
                selectedStudent.setName(nameField.getText());
                selectedStudent.setEmail(emailField.getText());
                table.refresh();
                return selectedStudent;
            }
            return null;
        });

        dialog.showAndWait();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

**2.2. Test et validation**
- Exécutez l'application, ajoutez, modifiez et supprimez des étudiants pour vérifier le bon fonctionnement de l'application.
- Corrigez les éventuels bugs et améliorez l'interface utilisateur selon les retours.

