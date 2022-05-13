public class game {


			public int[][] mas;
			int napr;
			private int gX,gY;
			public int kol;
			public int new_napr;
			private int dlina;
			public boolean endg;
			private void povorot()
			{
				if(Math.abs((new_napr-napr))!=2)
				{
					napr=new_napr;
				}
			}
			public game()
			{
				mas = new int[30][30];
			}
			private void make_new()
			{
				while(true)
				{
					int x=(int)(Math.random()*30);
					int y=(int)(Math.random()*30);
					if (mas[y][x]==0);
					{
						mas[y][x]=-1;
						break;
					}
				}
				}
			
			public void start()
			{
				for(int i=0;i<30;i++) {
					for(int j=0;j<30;j++) {
						mas[i][j]=0;
					}
				}
				napr = 0;
				kol=0;
				mas[14][14]=1;
				mas[14][15]=2;
				mas[14][16]=3;
				dlina=3;
				gX=gY=14;
				endg=false;
				make_new();
			}
			
	
public int peremGolova()
{
	
	if(napr==0)
	{
		if((gX-1)>=0)
			gX--;
		else
			gX=29;
	}
	else if (napr==1)
	{
		if((gY-1)>=0)
		    gY--;
		else
			gY=29;
	}
	else if(napr==2)
	{
		if((gX+1)<=29)
			gX++;
		else
			gX=0;
	}
	else if (napr==3)
	{
		if((gY+1)<=29)
			gY++;
		else
			gY=0;
	}
	int rez=0;
	if(mas[gY][gX]==-1)rez=1;
	else if (mas[gY][gX]==0)rez=2;
	else if(mas[gY][gX]>0)rez=3;
	mas[gY][gX]=-2;
	return rez;
}
public void perem(){
	int flag = peremGolova();
	if(flag==3)endg=true;
	for(int i=0; i<30; i++)
	{
		for(int j=0; j<30; j++)
		{
			if(mas[i][j]>0)mas[i][j]++;
			else if (mas[i][j]==-2)mas[i][j]=1;
			if(flag!=1)
			{
				if(mas[i][j]==(dlina+1))mas[i][j]=0;
			}
		}
	}
	if(flag==1)
	{
		dlina++;
		make_new();
		kol+=10;
	}
	povorot();

}
}
import java.awt.event.*;
import javax.swing.*;
import java.awt.*;
import javax.imageio.*;
import java.io.*;
public class zmeika {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		myFrame okno = new myFrame();

		}

	}
class myFrame extends JFrame
{
	public myFrame()
	{
		myPanel pan = new myPanel();
		Container cont = getContentPane();
		cont.add(pan);
		setTitle("Игра\"Змейка\"");
		setBounds(0,0,800,650);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setResizable(false);
		setVisible(true);
	}
}
class myPanel extends JPanel
{
	private myPanel pan;
	private game myGame;
	private Timer tmDraw,tmUpdate;
	private Image fon,telo,golova,ob,endg;
	private JLabel ib;
	private JButton btn1,btn2;
	private class myKey implements KeyListener
	{
		public void keyPressed(KeyEvent e)
		{
			int key = e.getKeyCode();
			if (key==KeyEvent.VK_LEFT)myGame.new_napr=0;
			else if (key==KeyEvent.VK_UP)myGame.new_napr=1;
			else if (key==KeyEvent.VK_RIGHT)myGame.new_napr=2;
			else if (key==KeyEvent.VK_DOWN)myGame.new_napr=3;
		}
		public void keyReleased(KeyEvent e) {}
		public void keyTyped(KeyEvent e) {}
	}
	

public myPanel()
{
	pan = this;
	this.addKeyListener(new myKey());
	this.setFocusable(true);
	try
	{
		fon=ImageIO.read(new File("c:\\zmeika\\fon.png"));
		telo = ImageIO.read(new File("c:\\zmeika\\telo.png"));
		golova = ImageIO.read(new File("c:\\zmeika\\golova.png"));
		ob=ImageIO.read(new File("c:\\zmeika\\ob.png"));
		endg=ImageIO.read(new File("c:\\zmeika\\end.png"));
	}
	catch (Exception ex) {}
	myGame = new game();
	myGame.start();
	tmUpdate=new Timer(100,new ActionListener() {

		
		public void actionPerformed(ActionEvent arg0) {
			if (myGame.endg==false)
			{
				myGame.perem();
			}
			ib.setText("Счёт: " + myGame.kol);
		
			
		}
		
	});
	tmUpdate.start();
	tmDraw = new Timer(20,new ActionListener() {
		@Override
		public void actionPerformed(ActionEvent arg0) {
			repaint();
		}
	});
	tmDraw.start();
	setLayout(null);
	ib=new JLabel("Счет: 0");
	ib.setForeground(Color.WHITE);
	ib.setFont(new Font("serif",0,30));
	ib.setBounds(630,200,150,50);
	add(ib);
	btn1 = new JButton();
	btn1.setText("Новая игра");
	btn1.setForeground(Color.BLUE);
	btn1.setFont(new Font("serif",0,20));
	btn1.setBounds(630,30,150,50);
	btn1.addActionListener(new ActionListener() {
		public void actionPerformed(ActionEvent argmn0) {
			myGame.start();
			btn1.setFocusable(false);
			btn2.setFocusable(false);
			pan.setFocusable(true);
		}
	});
	add(btn1);
	btn2=new JButton();
	btn2.setText("Выход");
	btn2.setForeground(Color.RED);
	btn2.setFont(new Font("serif",0,20));
	btn2.setBounds(630,100,150,50);
	btn2.addActionListener(new ActionListener() {;
	public void actionPerformed(ActionEvent arg0) {
			System.exit(0);
		}

	});
	add(btn2);
}
	public void paintComponent(Graphics gr)
	{
		super.paintComponent(gr);
		gr.drawImage(fon, 0, 0, 800,650,null );
		for(int i=0;i<30;i++){
			for(int j=0;j<30;j++){
				if (myGame.mas[i][j]!=0)
				{
					if(myGame.mas[i][j]==1)
					{
						gr.drawImage(golova, 10+j*20,10+i*20,20,20,null);
					}
					else if (myGame.mas[i][j]==-1)
					{
						gr.drawImage(ob, 10+j*20, 10+i*20,20,20,null);
					}
					else if (myGame.mas[i][j]==2)
					{
						gr.drawImage(telo, 10+j*20, 10+i*20, 20, 20,null);
					}
				}
				
			}
			if(myGame.endg == true)
			{
				gr.drawImage(endg, 250, 200, 200, 100, null);
			}
		}
		gr.setColor(Color.BLUE);
		for(int i=0;i<=30;i++)
		{
			gr.drawLine(10+i*20,10,10+i*20,610);
			gr.drawLine(10, 10+i*20, 610, 10+i*20);
		}
		
	
		
}
}

