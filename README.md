# CODECLAUSE
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Your Recipe Box</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background-color: #b3d9e3;
    }
    header {
      background-color: #001f87;
      color: white;
      padding: 2rem;
      text-align: center;
    }
    header h1 {
      margin: 0;
      font-size: 2.2rem;
    }
    header p {
      margin-top: 0.5rem;
    }
    main {
      padding: 2rem;
    }
    .top-bar {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .top-bar h2 {
      margin: 0;
    }
    .add-btn {
      background-color: #3498db;
      color: white;
      border: none;
      padding: 0.5rem 1rem;
      border-radius: 6px;
      cursor: pointer;
    }
    .empty-message {
      margin-top: 1rem;
      color: #f8f1f1;
    }
    .recipe-form {
      display: none;
      margin-top: 2rem;
      background-color: white;
      padding: 1rem;
      border-radius: 8px;
    }
    input, textarea {
      width: 100%;
      padding: 0.5rem;
      margin-bottom: 1rem;
      border: 1px solid #ccc;
      border-radius: 4px;
    }
    button.submit-btn {
      background-color: #28a745;
      color: white;
      border: none;
      padding: 0.5rem 1rem;
      border-radius: 4px;
      cursor: pointer;
    }
    .footer {
      background-color: #6ba6ae;
      color: white;
      text-align: center;
      padding: 1rem;
      position: relative;
      bottom: 0;
      width: 100%;
    }
  </style>
</head>
<body>
  <header>
    <h1>Your Recipe Box</h1>
    <p>Cherished family recipes & kitchen creations</p>
  </header>

  <main>
    <div class="top-bar">
      <h2>Your Recipes</h2>
      <button class="add-btn" onclick="toggleForm()">+ Add Recipe</button>
    </div>
    <p class="empty-message" id="emptyMessage">
      Your recipe box is empty! Let's fill it with deliciousness - click "Add Recipe" to begin your collection!
    </p>

    <form class="recipe-form" id="recipeForm">
      <input type="text" id="title" placeholder="Recipe Title" required />
      <textarea id="ingredients" placeholder="Ingredients (comma-separated)" required></textarea>
      <textarea id="instructions" placeholder="Instructions" required></textarea>
      <input type="url" id="image" placeholder="Image URL (optional)" />
      <button type="submit" class="submit-btn">Save Recipe</button>
    </form>

    <div id="recipeList"></div>
  </main>

  <footer class="footer">
    Made with ❤️ in Your Kitchen - Share the love, one recipe at a time
  </footer>

  <script>
    const form = document.getElementById("recipeForm");
    const recipeList = document.getElementById("recipeList");
    const emptyMessage = document.getElementById("emptyMessage");

    function toggleForm() {
      form.style.display = form.style.display === "none" || form.style.display === "" ? "block" : "none";
    }

    form.addEventListener("submit", function (e) {
      e.preventDefault();
      const title = document.getElementById("title").value;
      const ingredients = document.getElementById("ingredients").value;
      const instructions = document.getElementById("instructions").value;
      const image = document.getElementById("image").value;

      const recipe = { title, ingredients, instructions, image };
      let recipes = JSON.parse(localStorage.getItem("recipes")) || [];
      recipes.push(recipe);
      localStorage.setItem("recipes", JSON.stringify(recipes));

      form.reset();
      toggleForm();
      displayRecipes();
    });

    function displayRecipes() {
      recipeList.innerHTML = "";
      const recipes = JSON.parse(localStorage.getItem("recipes")) || [];

      if (recipes.length === 0) {
        emptyMessage.style.display = "block";
        return;
      } else {
        emptyMessage.style.display = "none";
      }

      recipes.forEach((r, index) => {
        const card = document.createElement("div");
        card.className = "recipe-card";
        card.style.background = "white";
        card.style.marginTop = "1rem";
        card.style.padding = "1rem";
        card.style.borderRadius = "8px";

        card.innerHTML = `
          <h3>${r.title}</h3>
          <p><strong>Ingredients:</strong> ${r.ingredients}</p>
          <p><strong>Instructions:</strong> ${r.instructions}</p>
          ${r.image ? `<img src="${r.image}" alt="${r.title}" style="max-width:100%; margin-top:1rem;" />` : ""}
          <button onclick="deleteRecipe(${index})" style="margin-top:1rem; background:#dc3545; color:white; border:none; padding:0.3rem 0.8rem; border-radius:4px;">Delete</button>
        `;
        recipeList.appendChild(card);
      });
    }

    function deleteRecipe(index) {
      let recipes = JSON.parse(localStorage.getItem("recipes"));
      recipes.splice(index, 1);
      localStorage.setItem("recipes", JSON.stringify(recipes));
      displayRecipes();
    }

    displayRecipes();
  </script>
</body>
</html>

