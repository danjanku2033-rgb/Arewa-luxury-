import React, { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { ShoppingCart, Store, Trash2 } from "lucide-react";
import { motion } from "framer-motion";

export default function ArewaLuxuryMarketplace() {
  const [products, setProducts] = useState([
    { id: 1, name: "Hausa Traditional Cap", price: 15000, vendor: "Arewa Caps" },
    { id: 2, name: "Luxury Kaftan", price: 45000, vendor: "Northern Style" },
  ]);

  const [cart, setCart] = useState([]);
  const [newProduct, setNewProduct] = useState({
    name: "",
    price: "",
    vendor: "",
  });

  const addToCart = (product) => {
    setCart((prev) => [...prev, product]);
  };

  const removeFromCart = (index) => {
    setCart((prev) => prev.filter((_, i) => i !== index));
  };

  const addProduct = () => {
    if (!newProduct.name || !newProduct.price || !newProduct.vendor) return;

    const product = {
      id: Date.now(),
      name: newProduct.name,
      price: Number(newProduct.price),
      vendor: newProduct.vendor,
    };

    setProducts((prev) => [...prev, product]);
    setNewProduct({ name: "", price: "", vendor: "" });
  };

  const total = cart.reduce((sum, item) => sum + item.price, 0);

  return (
    <div className="min-h-screen bg-gray-100 p-6">
      <motion.h1
        initial={{ opacity: 0, y: -20 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ duration: 0.5 }}
        className="text-3xl font-bold mb-6"
      >
        Arewa Luxury Marketplace
      </motion.h1>

      <div className="grid md:grid-cols-3 gap-6">
        <div className="md:col-span-2">
          <h2 className="text-xl font-semibold mb-4">Products</h2>
          <div className="grid sm:grid-cols-2 gap-4">
            {products.map((product) => (
              <Card key={product.id} className="rounded-2xl shadow-md hover:shadow-lg transition">
                <CardContent className="p-4">
                  <h3 className="font-bold text-lg">{product.name}</h3>
                  <p className="text-sm text-gray-500">Sold by {product.vendor}</p>
                  <p className="text-lg font-semibold mt-2">₦{product.price}</p>
                  <Button
                    onClick={() => addToCart(product)}
                    className="mt-3 w-full"
                  >
                    <ShoppingCart className="w-4 h-4 mr-2" /> Add to Cart
                  </Button>
                </CardContent>
              </Card>
            ))}
          </div>
        </div>

        <div>
          <Card className="rounded-2xl shadow-md mb-6">
            <CardContent className="p-4">
              <h2 className="text-xl font-semibold mb-4 flex items-center gap-2">
                <ShoppingCart className="w-5 h-5" /> Cart
              </h2>

              {cart.length === 0 ? (
                <p className="text-gray-500">Cart is empty</p>
              ) : (
                <ul className="space-y-2">
                  {cart.map((item, index) => (
                    <li key={index} className="flex justify-between items-center">
                      <span>{item.name}</span>
                      <div className="flex items-center gap-2">
                        <span>₦{item.price}</span>
                        <Trash2
                          className="w-4 h-4 cursor-pointer"
                          onClick={() => removeFromCart(index)}
                        />
                      </div>
                    </li>
                  ))}
                </ul>
              )}

              <p className="mt-4 font-bold">Total: ₦{total}</p>
              <Button className="w-full mt-3">Checkout</Button>
            </CardContent>
          </Card>

          <Card className="rounded-2xl shadow-md">
            <CardContent className="p-4">
              <h2 className="text-xl font-semibold mb-4 flex items-center gap-2">
                <Store className="w-5 h-5" /> List Your Product
              </h2>

              <input
                type="text"
                placeholder="Product Name"
                value={newProduct.name}
                onChange={(e) =>
                  setNewProduct({ ...newProduct, name: e.target.value })
                }
                className="w-full border p-2 rounded mb-2"
              />

              <input
                type="number"
                placeholder="Price"
                value={newProduct.price}
                onChange={(e) =>
                  setNewProduct({ ...newProduct, price: e.target.value })
                }
                className="w-full border p-2 rounded mb-2"
              />

              <input
                type="text"
                placeholder="Vendor Name"
                value={newProduct.vendor}
                onChange={(e) =>
                  setNewProduct({ ...newProduct, vendor: e.target.value })
                }
                className="w-full border p-2 rounded mb-4"
              />

              <Button onClick={addProduct} className="w-full">
                Add Product
              </Button>
            </CardContent>
          </Card>
        </div>
      </div>
    </div>
  );
}
