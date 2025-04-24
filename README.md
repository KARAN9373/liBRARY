import React, { useState } from "react";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";

export default function BookLibraryManager() {
  const [search, setSearch] = useState("");

  const [books, setBooks] = useState([
    {
      id: 1,
      title: "The Alchemist",
      author: "Paulo Coelho",
      category: "Fiction",
      borrowed: false,
      history: []
    },
    {
      id: 2,
      title: "Atomic Habits",
      author: "James Clear",
      category: "Self Help",
      borrowed: false,
      history: []
    },
    {
      id: 3,
      title: "1984",
      author: "George Orwell",
      category: "Dystopian",
      borrowed: false,
      history: []
    }
  ]);

  const handleBorrow = (id) => {
    const borrowDate = new Date().toLocaleString();
    const updatedBooks = books.map(book => {
      if (book.id === id && !book.borrowed) {
        return {
          ...book,
          borrowed: true,
          history: [...book.history, borrowDate]
        };
      }
      return book;
    });
    setBooks(updatedBooks);
  };

  const filteredBooks = books.filter(book =>
    book.title.toLowerCase().includes(search.toLowerCase()) ||
    book.author.toLowerCase().includes(search.toLowerCase()) ||
    book.category.toLowerCase().includes(search.toLowerCase())
  );

  return (
    <div className="p-6 max-w-4xl mx-auto">
      <h1 className="text-3xl font-bold mb-4">My Personal Book Library</h1>
      <Input
        placeholder="Search by title, author, or category..."
        value={search}
        onChange={(e) => setSearch(e.target.value)}
        className="mb-6"
      />
      <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
        {filteredBooks.map(book => (
          <Card key={book.id} className="bg-white shadow rounded-xl p-4">
            <CardContent>
              <h2 className="text-xl font-semibold">{book.title}</h2>
              <p className="text-sm text-gray-700">Author: {book.author}</p>
              <p className="text-sm text-gray-700">Category: {book.category}</p>
              <p className="text-sm text-gray-700">Status: {book.borrowed ? "Borrowed" : "Available"}</p>
              <Button
                className="mt-2"
                onClick={() => handleBorrow(book.id)}
                disabled={book.borrowed}
              >
                {book.borrowed ? "Already Borrowed" : "Borrow Book"}
              </Button>
              {book.history && book.history.length > 0 && (
                <div className="mt-2 text-xs text-gray-600">
                  <p className="font-semibold">Borrow History:</p>
                  <ul>
                    {book.history.map((entry, index) => (
                      <li key={index}>- {entry}</li>
                    ))}
                  </ul>
                </div>
              )}
            </CardContent>
          </Card>
        ))}
      </div>
    </div>
  );
}
