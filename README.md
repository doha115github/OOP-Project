# OOP-Projectimport javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.awt.event.ActionListener;
import java.awt.event.ActionEvent;
import java.io.*;

public class App {
    public static void main(String[] args) throws Exception {
        // System.out.println("Hello, World!");
        new GuiEx4();
        new TextEditor();
    }
}
class TextEditor extends JFrame implements ActionListener{
    JFrame frame;
    JTextArea textarea;
    JScrollPane scroll;
    JMenu file,edit,themes,tools;
    JMenuBar menuBar;
    JMenuItem save,open,New,close,cut,copy,paste,selectAll,darkTheme,defaultTheme,fontColor,fontSize;

    <keyListener> TextEditor() {
        frame = new JFrame("TextEditor");
        ImageIcon image=new ImageIcon("C:\\Users\\Dell\\3D Objects\\textlogo.jpg");
         frame.setIconImage(image.getImage());
        //setting Textarea
        textarea = new JTextArea(32,88);
        frame.add(textarea);

        //adding scrollPane
        scroll = new JScrollPane(textarea);
        scroll.setHorizontalScrollBarPolicy(JScrollPane.HORIZONTAL_SCROLLBAR_AS_NEEDED);
        scroll.setVerticalScrollBarPolicy(JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED);
        frame.add(scroll);



        //creating MenuBar
        menuBar = new JMenuBar();

        //Naming the  Menus of MenuBar
        file = new JMenu("File");
        edit = new JMenu("Edit");
        themes = new JMenu("Themes");
        tools = new JMenu("Tools");

        //adding the Menus to MenuBar
        menuBar.add(file);
        menuBar.add(edit);
        menuBar.add(themes);
        menuBar.add(tools);
        frame.setJMenuBar(menuBar);

        //creating and adding further
        // menuItems to the Menu named File
        save = new JMenuItem("Save     |Ctrl S");
        open = new JMenuItem("Open    |Ctrl O");
        New = new JMenuItem( "New      |Ctrl N");
        close = new JMenuItem("Exit");
        file.add(New);
        file.add(open);
        file.add(save);
        file.add(close);
        //creating and adding further
        // menuItems to the Menu named Edit
        cut = new JMenuItem("Cut");
        copy = new JMenuItem("Copy             | Ctrl C");
        paste = new JMenuItem("Paste           | Ctrl V");
        selectAll = new JMenuItem("Select All    | Ctrl A");
        edit.add(cut);
        edit.add(copy);
        edit.add(paste);
        edit.add(selectAll);

        //creating and adding the themes to
        //theme Menu
        darkTheme = new JMenuItem("Dark Theme");
        defaultTheme = new JMenuItem("Default Theme");
        themes.add(darkTheme);
        themes.add(defaultTheme);

        //tools Menu
        fontSize = new JMenuItem("Font Size");
        fontColor = new JMenuItem("Font Color");
        tools.add(fontSize);
        tools.add(fontColor);

        //adding event listeners for cut , copy & paste
        cut.addActionListener(this);
        copy.addActionListener(this);
        paste.addActionListener(this);
        selectAll.addActionListener(this);
        open.addActionListener(this); //open the file
        save.addActionListener(this); //Save the file
        New.addActionListener(this); //Create the new document
        darkTheme.addActionListener(this); //dark theme
        defaultTheme.addActionListener(this); // default theme
        close.addActionListener(this); //close the window
        fontSize.addActionListener(this);// size of the font
        fontColor.addActionListener(this);// color of the font

        //creating and adding keyListeners for saving any file
        KeyListener k = new KeyListener() {
            @Override
            public void keyTyped(KeyEvent e) {

            }

            @Override
            public void keyPressed(KeyEvent e) {
                int keyCode = e.getKeyCode();
                if (keyCode == KeyEvent.VK_S && e.isControlDown()) {
                    saveTheFile();
                } else if (keyCode == KeyEvent.VK_O && e.isControlDown()) {
                    OpenAnyFile();
                } else if(keyCode== KeyEvent.VK_A && e.isControlDown()){
                    textarea.selectAll();
                }
                  else if(keyCode==KeyEvent.VK_N && e.isControlDown()){
                    textarea.setText("");
                }
            }
             @Override
             public void keyReleased(KeyEvent e) {
             }
         };
         //adding the key listener k to textArea
             textarea.addKeyListener(k);

         //adding window listeners for closing the window
        frame.addWindowListener(new WindowListener() {
            @Override
            public void windowOpened(WindowEvent e) {

            }
            @Override
            public void windowClosing(WindowEvent e) {
                int confirmExit = JOptionPane.showConfirmDialog(frame, "Do you want to exit?",
                        "Confirm Before Saving...", JOptionPane.YES_NO_OPTION);
                if (confirmExit == JOptionPane.YES_OPTION) {
                    frame.setFocusable(false);
                    frame.dispose();
                } else if (confirmExit == JOptionPane.NO_OPTION)
                { frame.setDefaultCloseOperation(JFrame.DO_NOTHING_ON_CLOSE);}
            }
            @Override
            public void windowClosed(WindowEvent e) { }
            @Override
            public void windowIconified(WindowEvent e) { }
            @Override
            public void windowDeiconified(WindowEvent e) { }
            @Override
            public void windowActivated(WindowEvent e) { }
            @Override
            public void windowDeactivated(WindowEvent e) { }
            });

        // setting the basic operations for frame
        frame.setSize(1000, 596);
        frame.setLayout(new FlowLayout());
        frame.setLocation(250, 100);
        frame.setResizable(false);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
       
        frame.setVisible(true);

    }
           @Override
           public void actionPerformed(ActionEvent e){
            //for performing cut ,copy and paste operations
            if(e.getSource()==cut)
                textarea.cut();
            if(e.getSource()==copy)
                textarea.copy();
            if(e.getSource()==paste)
                textarea.paste();
            if(e.getSource()==selectAll)
                textarea.selectAll();
            if(e.getSource()==open){
                OpenAnyFile(); }
            if(e.getSource()==fontSize){
                String sizeOfFont = JOptionPane.showInputDialog(frame,"Enter Font Size",JOptionPane.OK_CANCEL_OPTION);
                if (sizeOfFont != null){
                    int convertSizeOfFont = Integer.parseInt(sizeOfFont);
                    Font font = new Font(Font.SANS_SERIF,Font.PLAIN,convertSizeOfFont);
                    textarea.setFont(font);
                }
            }
            if(e.getSource()==fontColor){
                JColorChooser colorChooser=new JColorChooser();
                Color color=JColorChooser.showDialog(null, "Choose a color", Color.BLACK);

                textarea.setForeground(color);
            }
               //Save the file
               if (e.getSource()==save) saveTheFile();


               //New menu operations
               if (e.getSource()==New) textarea.setText("");


               //Exit from the window
               if (e.getSource()==close) System.exit(1);


               //for setting the themes
               if (e.getSource()==darkTheme){
                   textarea.setBackground(Color.DARK_GRAY);        //dark Theme
                   textarea.setForeground(Color.WHITE);
               }
               if (e.getSource() == defaultTheme){
                   textarea.setBackground(Color.WHITE);
                   textarea.setForeground(Color.black);
               } }
    public void OpenAnyFile(){
        JFileChooser chooseFile =new JFileChooser();
        int i=chooseFile.showOpenDialog(frame);
        if(i== JFileChooser.APPROVE_OPTION){
            File file =chooseFile.getSelectedFile();// selecting the file
            String filePath=file.getPath(); //getting the path of the selected file------
            String fileNameToShow = file.getName();//getting the name of selected file
            frame.setTitle(fileNameToShow);

            try{
                BufferedReader readFile= new BufferedReader(new FileReader(filePath));
                String tempString1 ="";
                String tempString2 ="";
                while((tempString1 = readFile.readLine())!=null)
                    tempString2 +=tempString1 + "\n";

                textarea.setText(tempString2);
                readFile.close();
            } catch(Exception ae){
                ae.printStackTrace();
            }
        } }
    //Save the file
    public void saveTheFile(){
        JFileChooser fileChooser=new JFileChooser();
        fileChooser.setCurrentDirectory(new File("."));

        int response=fileChooser.showSaveDialog(null);

        if(response == JFileChooser.APPROVE_OPTION){
            File file;
            PrintWriter fileOut=null;

            file =new File(fileChooser.getSelectedFile().getAbsolutePath());

            try{
                fileOut=new PrintWriter(file);
                fileOut.println(textarea.getText());
            }
            catch (FileNotFoundException e) {
                //TODO Auto-generated catch block
                e.printStackTrace();
            }
            finally{
                fileOut.close();
            }
        }
    }
    }
    class GuiEx4 extends JFrame{
	
        public GuiEx4() {
            // TODO Auto-generated constructor stub
            ImageIcon ic=new ImageIcon("D:\\SW Lecturer\\1st Subject Summer Semester OOP\\Eclipse Data\\E Workspace\\MyProj\\src\\4.jpg");
            JPanel jp=new JPanel();
            jp.setLayout(new BorderLayout());
            JLabel lb=new JLabel(ic);
    //		lb.setIcon(ic);
            jp.add(lb, BorderLayout.CENTER);
            
            JProgressBar pb=new JProgressBar();
            pb.setStringPainted(true);
            pb.setMaximum(0);
            pb.setMaximum(100);
            jp.add(pb, BorderLayout.SOUTH);
            
            add(jp);
            
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setContentPane(jp);
            setLocation(200,300);
            setVisible(true);
            
            setSize(400,300);
            
            for(int i=0; i<=1000; i++){
                pb.setValue(i);
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
                if (i == 100) {
                    setVisible(false);
                    this.dispose();
                    new TextEditor();
                    //call the class here which you want to show
                }
            }
            
        }
        
        
    }
