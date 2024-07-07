**Command** is behavioral design pattern that converts requests or simple operations into objects.

**Command.java**

```java
public abstract class Command {
    public Editor editor;
    private String backup;

    Command(Editor editor) {
        this.editor = editor;
    }

    void backup() {
        backup = editor.textField.getText();
    }

    public void undo() {
        editor.textField.setText(backup);
    }

    public abstract boolean execute();
}
```

**CopyCommand.java**

```java
public class CopyCommand extends Command {

    public CopyCommand(Editor editor) {
        super(editor);
    }

    @Override
    public boolean execute() {
        editor.clipboard = editor.textField.getSelectedText();
        return false;
    }
}
```

**PasteCommand.java**

```java
public class PasteCommand extends Command {

    public PasteCommand(Editor editor) {
        super(editor);
    }

    @Override
    public boolean execute() {
        if (editor.clipboard == null || editor.clipboard.isEmpty()) {
	        return false;
		}
        backup();
        editor.textField.insert(editor.clipboard, 
						        editor.textField.getCaretPosition());
        return true;
    }
}
```

**CutCommand.java**

```java
public class CutCommand extends Command {

    public CutCommand(Editor editor) {
        super(editor);
    }

    @Override
    public boolean execute() {
        if (editor.textField.getSelectedText().isEmpty()) {
	        return false;
	    }

        backup();
        String source = editor.textField.getText();
        editor.clipboard = editor.textField.getSelectedText();
        editor.textField.setText(cutString(source));
        return true;
    }

    private String cutString(String source) {
        String start = 
		        source.substring(0, editor.textField.getSelectionStart());
        String end = source.substring(editor.textField.getSelectionEnd());
        return start + end;
    }
}
```

**CommandHistory.java**

```java
public class CommandHistory {
    private Stack<Command> history = new Stack<>();

    public void push(Command c) {
        history.push(c);
    }

    public Command pop() {
        return history.pop();
    }

    public boolean isEmpty() { return history.isEmpty(); }
}
```

**Editor.java**

```java
public class Editor {
    private CommandHistory history = new CommandHistory();
    public JTextArea textField;
    public String clipboard;

    public void init() {
		Editor editor = this;
        executeCommand(new CopyCommand(editor));
        executeCommand(new CutCommand(editor));
        executeCommand(new PasteCommand(editor));
        undo();
    }

    private void executeCommand(Command command) {
        if (command.execute()) {
            history.push(command);
        }
    }

    private void undo() {
        if (history.isEmpty()) {
	        return;
	    }
        Command command = history.pop();
        if (command != null) {
            command.undo();
        }
    }
}
```

