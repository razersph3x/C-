using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace Nikson
{

    public class book
    {
        string avtor;
        string name;
        public enum Title { tematika1, tematika2, tematika3 };
        Title title;

        public void setStyle(Title title)
        {
            this.title = title;
        }
        public Title getStyle()
        {
            return this.title;
        }
        public string getName()
        {
            return this.name;
        }
        public void setName(string name)
        {
            this.name = name;
        }
        public string getAvtor()
        {
            return this.avtor;
        }
        public void setAvtor(string avtor)
        {
            this.avtor = avtor;
        }

    }

    class Program
    {
        static void Main(string[] args)
        {
            string[] StrBooks = System.IO.File.ReadAllLines("Список книг.txt");
            book[] books = new book[StrBooks.Length];
            for (int i = 0; i < StrBooks.Length; i++)
            {
                books[i] = new book();
            }
            for (int i = 0; i < StrBooks.Length; i++)
            {
                int schet = 0;
                for (int j = 0; j < StrBooks[i].Length; j++)
                {
                    if ((StrBooks[i][j] == ' ') && (schet == 0))
                    {
                        schet++;
                        books[i].setAvtor(StrBooks[i].Substring(0, j));
                    }
                    else if ((StrBooks[i][j] == ' ') && (schet == 1))
                    {
                        schet++;
                        books[i].setName(StrBooks[i].Substring(books[i].getAvtor().Length + 1, j - books[i].getAvtor().Length - 1));
                        books[i].setStyle((book.Title)Enum.Parse(typeof(book.Title), StrBooks[i].Substring(books[i].getAvtor().Length + books[i].getName().Length + 2, StrBooks[i].Length - (books[i].getAvtor().Length + books[i].getName().Length + 2))));
                    }
                }
            }

            Console.WriteLine("Введите количество наборов");
            int kolich = (Convert.ToInt32(Console.ReadLine()));
            for (int i = 1; i < kolich; i++)
            {
                using (FileStream fs = File.Create("набор" + (i) + ".txt")) ;
            }
            int chet = 1;
            for (int i = 0; i < 3; i++)
            {
                for (int j = 0; j < books.Length; j++)
                {
                    if ((int)books[j].getStyle() == i)
                    {
                        zapis(books, chet, j);
                        chet++;
                        if (chet > kolich) chet = 1;
                    }
                }
            }

            Console.ReadKey();
        }

        static void zapis(book[] books, int chet, int j)
        {
            System.IO.File.AppendAllText("набор" + chet + ".txt", (books[j].getAvtor() + " " + books[j].getName() + " " + books[j].getStyle() + "\n"));

        }

    }
}
