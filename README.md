# DataUtil
import java.awt.*;
import java.awt.event.*;
import java.util.Vector;
import javax.swing.*;
import javax.swing.event.*;
import javax.swing.table.*;

public class AddRemoveCells implements ActionListener {
	JTable table = null;
	// 此处用textarea是因为可以多行编辑添加panel，功能比textfield强大。
	TextArea textArea = new TextArea();
	String str = new String();
	DefaultTableModel defaultModel = null;

	public AddRemoveCells() {
		JFrame f = new JFrame();
		textArea = new TextArea();
		textArea.setBackground(Color.BLACK);
		textArea.setForeground(Color.GREEN);
		textArea.setText("发送中.........");
		JScrollPane scrollPane = new JScrollPane(textArea);
		String[] name = { "设备号", "参数号", "参数值", "时标", "目的地址", "端口号" };
		String[][] data = new String[2][6];
		int value = 1;
		// for(int i=0; i<data.length; i++){
		// for(int j=0; j<data.length ; j++)
		// data[j] = String.valueOf(value++);

		defaultModel = new DefaultTableModel(data, name);
		table = new JTable(defaultModel);
		table.setPreferredScrollableViewportSize(new Dimension(400, 150));
		JScrollPane s = new JScrollPane(table);
		// scrollPane.setBounds(10, 20, 380, 170);

		JPanel panel = new JPanel();
		JButton b = new JButton("显示帧");
		panel.add(b);
		b.addActionListener(this);
		b = new JButton("增加行");
		panel.add(b);
		b.addActionListener(this);
		b = new JButton("删除行");
		panel.add(b);
		b.addActionListener(this);
		// b = new JButton("删除列");
		// panel.add(b);
		// b.addActionListener(this);

		Container contentPane = f.getContentPane();
		contentPane.add(panel, BorderLayout.CENTER);

		contentPane.add(s, BorderLayout.NORTH);

		contentPane.add(scrollPane, BorderLayout.SOUTH);

		f.setTitle("发数工具");
		f.pack();
		f.setVisible(true);

		f.addWindowListener(new WindowAdapter() {
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			}
		});
	}

	/*
	 * 要删除列必须使用TableColumnModel界面定义的removeColumn()方法。因此我先由JTable类的
	 * getColumnModel()方法取得TableColumnModel对象，再由TableColumnModel的getColumn()方法取
	 * 得要删除列的TableColumn.此TableColumn对象当作是removeColumn()的参数。删除此列完毕后必须重新设置列数，也就是
	 * 使用DefaultTableModel的setColumnCount()方法来设置。
	 */
	public void actionPerformed(ActionEvent e) {
		if (e.getActionCommand().equals("显示帧")) {
			int row = table.getRowCount();
		}
		int col = table.getColumnCount();
		int row = table.getRowCount();
		System.out.println(row);
		textArea.setText("");
		if (row != 0) {
			// String[][] values=new String [row][col];
			for (int i = 0; i < row; i++) {
				str = "";
				for (int j = 0; j < col; j++)
					// values[i][j]=(String)table.getValueAt(row, col);
					str = str + (String) table.getValueAt(i, j) + " ";
				textArea.append(str + "\n");
			}
			if (e.getActionCommand().equals("增加行"))
				defaultModel.addRow(new Vector());
			// if(e.getActionCommand().equals("删除列")){
			// int columncount = defaultModel.getColumnCount()-1;
			// if(columncount >= 0) { //若columncount<0代表已经没有任何列了。
			// TableColumnModel columnModel = table.getColumnModel();
			// TableColumn tableColumn = columnModel.getColumn(columncount);
			// columnModel.removeColumn(tableColumn);
			// defaultModel.setColumnCount(columncount);
			// }
			// }
			if (e.getActionCommand().equals("删除行")) {
				int rowcount = defaultModel.getRowCount() - 1;// getRowCount返回行数，rowcount<0代表已经没有任何行了。
				if (rowcount >= 0) {
					defaultModel.removeRow(rowcount);
					defaultModel.setRowCount(rowcount);
					/*
					 * 删除行比较简单，只要用DefaultTableModel的removeRow()方法即可。//删除行完毕后必须重新设置列数
					 * ，也就是使用DefaultTableModel的setRowCount()方法来设置。
					 */
				}
			}
			table.revalidate();
		}
	}

	public static void main(String args[]) {
		new AddRemoveCells();
	}
}
