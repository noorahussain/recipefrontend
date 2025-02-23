import React, { useState, useEffect } from 'react';
import axios from 'axios';
import { Card, CardContent } from '@/components/ui/card';
import { Button } from '@/components/ui/button';

const RecipeSharingApp = () => {
  const [recipes, setRecipes] = useState([]);
  const [newRecipe, setNewRecipe] = useState({ title: '', ingredients: '', instructions: '' });

  useEffect(() => {
    fetchRecipes();
  }, []);

  const fetchRecipes = async () => {
    try {
      const response = await axios.get('http://localhost:5000/api/recipes');
      setRecipes(response.data);
    } catch (error) {
      console.error('Error fetching recipes:', error);
    }
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      await axios.post('http://localhost:5000/api/recipes', newRecipe);
      setNewRecipe({ title: '', ingredients: '', instructions: '' });
      fetchRecipes();
    } catch (error) {
      console.error('Error adding recipe:', error);
    }
  };

  return (
    <div className="min-h-screen bg-gray-100 p-6">
      <h1 className="text-3xl font-bold text-center mb-6">Recipe Sharing Platform</h1>
      <form onSubmit={handleSubmit} className="bg-white p-6 rounded-2xl shadow-lg mb-6">
        <input
          type="text"
          placeholder="Recipe Title"
          value={newRecipe.title}
          onChange={(e) => setNewRecipe({ ...newRecipe, title: e.target.value })}
          className="w-full p-2 mb-4 rounded-xl border"
          required
        />
        <textarea
          placeholder="Ingredients"
          value={newRecipe.ingredients}
          onChange={(e) => setNewRecipe({ ...newRecipe, ingredients: e.target.value })}
          className="w-full p-2 mb-4 rounded-xl border"
          required
        ></textarea>
        <textarea
          placeholder="Instructions"
          value={newRecipe.instructions}
          onChange={(e) => setNewRecipe({ ...newRecipe, instructions: e.target.value })}
          className="w-full p-2 mb-4 rounded-xl border"
          required
        ></textarea>
        <Button type="submit" className="w-full bg-blue-500 text-white py-2 rounded-xl">Add Recipe</Button>
      </form>
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        {recipes.map((recipe) => (
          <Card key={recipe._id} className="bg-white rounded-2xl shadow-lg">
            <CardContent>
              <h2 className="text-xl font-bold mb-2">{recipe.title}</h2>
              <p className="mb-2"><strong>Ingredients:</strong> {recipe.ingredients}</p>
              <p><strong>Instructions:</strong> {recipe.instructions}</p>
            </CardContent>
          </Card>
        ))}
      </div>
    </div>
  );
};

export default RecipeSharingApp;
