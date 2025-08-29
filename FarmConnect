<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FarmConnect - Low-Bandwidth Farming App</title>
    <!-- Tailwind CSS for styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons for the React components -->
    <script src="https://unpkg.com/lucide@latest/dist/umd/lucide.js"></script>
    <!-- React and ReactDOM for rendering -->
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <!-- Babel for JSX transformation -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <style>
        /* Custom styles for mobile optimization */
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            -webkit-tap-highlight-color: transparent;
        }
        .post-image {
            max-height: 300px;
            object-fit: cover;
            width: 100%;
        }
        /* Animation for better UX */
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        .fade-in {
            animation: fadeIn 0.3s ease-in;
        }
        /* Ensure mobile-friendly touch targets */
        button, .nav-item {
            min-height: 44px;
            min-width: 44px;
        }
    </style>
</head>
<body class="bg-gray-50">
    <div id="root" class="min-h-screen"></div>

    <script type="text/babel">
        // Main React component
        const { useState, useRef, useEffect } = React;

        const FarmingApp = () => {
            // Define icon components
            const Camera = () => lucide.createIcon('camera');
            const User = () => lucide.createIcon('user');
            const Home = () => lucide.createIcon('home');
            const Plus = () => lucide.createIcon('plus');
            const Search = () => lucide.createIcon('search');
            const Heart = () => lucide.createIcon('heart');
            const MessageCircle = () => lucide.createIcon('message-circle');
            const Share2 = () => lucide.createIcon('share-2');
            const Edit3 = () => lucide.createIcon('edit-3');
            const Save = () => lucide.createIcon('save');
            const X = () => lucide.createIcon('x');

            const [currentUser, setCurrentUser] = useState(null);
            const [activeTab, setActiveTab] = useState('home');
            const [signupData, setSignupData] = useState({
                name: '',
                location: '',
                farmType: '',
                experience: ''
            });
            const [posts, setPosts] = useState([
                {
                    id: 1,
                    user: { name: 'Maria Santos', location: 'Philippines', avatar: '👩‍🌾' },
                    content: 'Harvested my first batch of organic tomatoes! The yield exceeded expectations.',
                    image: null,
                    likes: 12,
                    comments: 3,
                    timestamp: '2 hours ago',
                    type: 'story'
                },
                {
                    id: 2,
                    user: { name: 'John Kwame', location: 'Ghana', avatar: '👨‍🌾' },
                    content: 'Sharing my drought-resistant maize variety. Seeds available for exchange.',
                    image: null,
                    likes: 8,
                    comments: 5,
                    timestamp: '5 hours ago',
                    type: 'resource'
                }
            ]);
            const [showSignup, setShowSignup] = useState(true);
            const [showNewPost, setShowNewPost] = useState(false);
            const [editingProfile, setEditingProfile] = useState(false);
            const [newPost, setNewPost] = useState({ content: '', type: 'story', image: null });
            const [profileEdit, setProfileEdit] = useState({
                name: '',
                location: '',
                farmType: '',
                experience: ''
            });
            const fileInputRef = useRef(null);

            const handleSignup = () => {
                if (signupData.name && signupData.location && signupData.farmType && signupData.experience) {
                    const userData = {
                        ...signupData,
                        avatar: '👤'
                    };
                    setCurrentUser(userData);
                    setShowSignup(false);
                    
                    // Save to localStorage for persistence
                    localStorage.setItem('currentUser', JSON.stringify(userData));
                }
            };

            const handleImageUpload = (e) => {
                const file = e.target.files[0];
                if (file && file.size <= 1048576) {
                    const reader = new FileReader();
                    reader.onload = (e) => {
                        setNewPost(prev => ({ ...prev, image: e.target.result }));
                    };
                    reader.readAsDataURL(file);
                } else {
                    alert('File size must be less than 1MB for low-bandwidth optimization');
                }
            };

            const handleCreatePost = () => {
                if (newPost.content.trim()) {
                    const post = {
                        id: posts.length + 1,
                        user: currentUser,
                        content: newPost.content,
                        image: newPost.image,
                        likes: 0,
                        comments: 0,
                        timestamp: 'Just now',
                        type: newPost.type
                    };
                    setPosts([post, ...posts]);
                    setNewPost({ content: '', type: 'story', image: null });
                    setShowNewPost(false);
                    
                    // Save to localStorage
                    const updatedPosts = [post, ...posts];
                    localStorage.setItem('posts', JSON.stringify(updatedPosts));
                }
            };

            const handleLike = (postId) => {
                const updatedPosts = posts.map(post => 
                    post.id === postId ? { ...post, likes: post.likes + 1 } : post
                );
                setPosts(updatedPosts);
                localStorage.setItem('posts', JSON.stringify(updatedPosts));
            };

            const updateProfile = () => {
                const updatedUser = {
                    ...currentUser,
                    name: profileEdit.name || currentUser.name,
                    location: profileEdit.location || currentUser.location,
                    farmType: profileEdit.farmType || currentUser.farmType,
                    experience: profileEdit.experience || currentUser.experience
                };
                setCurrentUser(updatedUser);
                setEditingProfile(false);
                localStorage.setItem('currentUser', JSON.stringify(updatedUser));
            };

            const startEditProfile = () => {
                setProfileEdit({
                    name: currentUser.name,
                    location: currentUser.location,
                    farmType: currentUser.farmType,
                    experience: currentUser.experience
                });
                setEditingProfile(true);
            };

            // Load data from localStorage on component mount
            useEffect(() => {
                const savedUser = localStorage.getItem('currentUser');
                const savedPosts = localStorage.getItem('posts');
                
                if (savedUser) {
                    setCurrentUser(JSON.parse(savedUser));
                    setShowSignup(false);
                }
                
                if (savedPosts) {
                    setPosts(JSON.parse(savedPosts));
                }
            }, []);

            if (showSignup) {
                return (
                    <div className="min-h-screen bg-green-50 flex items-center justify-center p-4">
                        <div className="bg-white rounded-lg shadow-lg p-6 w-full max-w-md fade-in">
                            <div className="text-center mb-6">
                                <h1 className="text-2xl font-bold text-green-700 mb-2">🌾 FarmConnect</h1>
                                <p className="text-gray-600">Join the farming community</p>
                            </div>
                            
                            <div className="space-y-4">
                                <div>
                                    <label className="block text-sm font-medium text-gray-700 mb-1">Full Name</label>
                                    <input 
                                        type="text" 
                                        value={signupData.name}
                                        onChange={(e) => setSignupData(prev => ({ ...prev, name: e.target.value }))}
                                        className="w-full px-3 py-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-green-500 focus:border-transparent"
                                        placeholder="Enter your name"
                                    />
                                </div>
                                
                                <div>
                                    <label className="block text-sm font-medium text-gray-700 mb-1">Location</label>
                                    <input 
                                        type="text" 
                                        value={signupData.location}
                                        onChange={(e) => setSignupData(prev => ({ ...prev, location: e.target.value }))}
                                        className="w-full px-3 py-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-green-500 focus:border-transparent"
                                        placeholder="City, Country"
                                    />
                                </div>
                                
                                <div>
                                    <label className="block text-sm font-medium text-gray-700 mb-1">Farm Type</label>
                                    <select 
                                        value={signupData.farmType}
                                        onChange={(e) => setSignupData(prev => ({ ...prev, farmType: e.target.value }))}
                                        className="w-full px-3 py-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-green-500 focus:border-transparent"
                                    >
                                        <option value="">Select farm type</option>
                                        <option value="vegetables">Vegetables</option>
                                        <option value="grains">Grains</option>
                                        <option value="fruits">Fruits</option>
                                        <option value="livestock">Livestock</option>
                                        <option value="mixed">Mixed Farming</option>
                                        <option value="other">Other</option>
                                    </select>
                                </div>
                                
                                <div>
                                    <label className="block text-sm font-medium text-gray-700 mb-1">Experience</label>
                                    <select 
                                        value={signupData.experience}
                                        onChange={(e) => setSignupData(prev => ({ ...prev, experience: e.target.value }))}
                                        className="w-full px-3 py-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-green-500 focus:border-transparent"
                                    >
                                        <option value="">Years of experience</option>
                                        <option value="beginner">Less than 2 years</option>
                                        <option value="intermediate">2-5 years</option>
                                        <option value="experienced">5-10 years</option>
                                        <option value="expert">More than 10 years</option>
                                    </select>
                                </div>
                                
                                <button 
                                    onClick={handleSignup}
                                    className="w-full bg-green-600 text-white py-2 px-4 rounded-md hover:bg-green-700 transition-colors"
                                >
                                    Join FarmConnect
                                </button>
                            </div>
                        </div>
                    </div>
                );
            }

            return (
                <div className="min-h-screen bg-gray-50">
                    <header className="bg-green-600 text-white p-4 sticky top-0 z-10 shadow-md">
                        <div className="flex items-center justify-between">
                            <h1 className="text-xl font-bold">🌾 FarmConnect</h1>
                            <div className="flex items-center space-x-2">
                                <Search className="w-5 h-5" />
                                <span className="text-sm">{currentUser?.name}</span>
                            </div>
                        </div>
                    </header>

                    <main className="pb-16">
                        {activeTab === 'home' && (
                            <div className="max-w-md mx-auto">
                                <div className="space-y-4 p-4">
                                    {posts.map(post => (
                                        <div key={post.id} className="bg-white rounded-lg shadow-sm border fade-in">
                                            <div className="p-4">
                                                <div className="flex items-center space-x-3 mb-3">
                                                    <div className="text-2xl">{post.user.avatar}</div>
                                                    <div className="flex-1">
                                                        <h3 className="font-semibold text-gray-900">{post.user.name}</h3>
                                                        <p className="text-sm text-gray-500">{post.user.location} • {post.timestamp}</p>
                                                    </div>
                                                    <span className={`px-2 py-1 rounded-full text-xs ${
                                                        post.type === 'story' ? 'bg-blue-100 text-blue-800' : 'bg-orange-100 text-orange-800'
                                                    }`}>
                                                        {post.type}
                                                    </span>
                                                </div>
                                                
                                                <p className="text-gray-800 mb-3">{post.content}</p>
                                                
                                                {post.image && (
                                                    <img 
                                                        src={post.image} 
                                                        alt="Post content" 
                                                        className="w-full rounded-lg mb-3 post-image"
                                                    />
                                                )}
                                                
                                                <div className="flex items-center justify-between text-gray-500">
                                                    <button 
                                                        onClick={() => handleLike(post.id)}
                                                        className="flex items-center space-x-1 hover:text-red-500 transition-colors"
                                                    >
                                                        <Heart className="w-4 h-4" />
                                                        <span>{post.likes}</span>
                                                    </button>
                                                    <button className="flex items-center space-x-1 hover:text-blue-500 transition-colors">
                                                        <MessageCircle className="w-4 h-4" />
                                                        <span>{post.comments}</span>
                                                    </button>
                                                    <button className="flex items-center space-x-1 hover:text-green-500 transition-colors">
                                                        <Share2 className="w-4 h-4" />
                                                        <span>Share</span>
                                                    </button>
                                                </div>
                                            </div>
                                        </div>
                                    ))}
                                </div>
                            </div>
                        )}

                        {activeTab === 'profile' && (
                            <div className="max-w-md mx-auto p-4">
                                <div className="bg-white rounded-lg shadow-sm border p-6 relative fade-in">
                                    {editingProfile ? (
                                        <div className="space-y-4">
                                            <div className="text-center mb-4">
                                                <div className="text-6xl mb-2">{currentUser.avatar}</div>
                                                <button
                                                    onClick={() => setEditingProfile(false)}
                                                    className="absolute top-4 right-4 text-gray-400 hover:text-gray-600"
                                                >
                                                    <X className="w-5 h-5" />
                                                </button>
                                            </div>
                                            
                                            <div>
                                                <label className="block text-sm font-medium text-gray-700 mb-1">Name</label>
                                                <input 
                                                    type="text" 
                                                    value={profileEdit.name}
                                                    onChange={(e) => setProfileEdit(prev => ({ ...prev, name: e.target.value }))}
                                                    className="w-full px-3 py-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-green-500"
                                                />
                                            </div>
                                            
                                            <div>
                                                <label className="block text-sm font-medium text-gray-700 mb-1">Location</label>
                                                <input 
                                                    type="text" 
                                                    value={profileEdit.location}
                                                    onChange={(e) => setProfileEdit(prev => ({ ...prev, location: e.target.value }))}
                                                    className="w-full px-3 py-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-green-500"
                                                />
                                            </div>
                                            
                                            <div>
                                                <label className="block text-sm font-medium text-gray-700 mb-1">Farm Type</label>
                                                <select 
                                                    value={profileEdit.farmType}
                                                    onChange={(e) => setProfileEdit(prev => ({ ...prev, farmType: e.target.value }))}
                                                    className="w-full px-3 py-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-green-500"
                                                >
                                                    <option value="vegetables">Vegetables</option>
                                                    <option value="grains">Grains</option>
                                                    <option value="fruits">Fruits</option>
                                                    <option value="livestock">Livestock</option>
                                                    <option value="mixed">Mixed Farming</option>
                                                    <option value="other">Other</option>
                                                </select>
                                            </div>
                                            
                                            <div>
                                                <label className="block text-sm font-medium text-gray-700 mb-1">Experience</label>
                                                <select 
                                                    value={profileEdit.experience}
                                                    onChange={(e) => setProfileEdit(prev => ({ ...prev, experience: e.target.value }))}
                                                    className="w-full px-3 py-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-green-500"
                                                >
                                                    <option value="beginner">Less than 2 years</option>
                                                    <option value="intermediate">2-5 years</option>
                                                    <option value="experienced">5-10 years</option>
                                                    <option value="expert">More than 10 years</option>
                                                </select>
                                            </div>
                                            
                                            <button 
                                                onClick={updateProfile}
                                                className="w-full bg-green-600 text-white py-2 px-4 rounded-md hover:bg-green-700 flex items-center justify-center space-x-2"
                                            >
                                                <Save className="w-4 h-4" />
                                                <span>Save Changes</span>
                                            </button>
                                        </div>
                                    ) : (
                                        <div className="text-center">
                                            <div className="text-6xl mb-4">{currentUser.avatar}</div>
                                            <h2 className="text-xl font-bold text-gray-900 mb-2">{currentUser.name}</h2>
                                            <p className="text-gray-600 mb-1">{currentUser.location}</p>
                                            <p className="text-gray-600 mb-1">Farm: {currentUser.farmType}</p>
                                            <p className="text-gray-600 mb-6">Experience: {currentUser.experience}</p>
                                            
                                            <button 
                                                onClick={startEditProfile}
                                                className="bg-green-600 text-white px-6 py-2 rounded-md hover:bg-green-700 flex items-center space-x-2 mx-auto"
                                            >
                                                <Edit3 className="w-4 h-4" />
                                                <span>Edit Profile</span>
                                            </button>
                                        </div>
                                    )}
                                </div>
                            </div>
                        )}
                    </main>

                    {showNewPost && (
                        <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4 z-20">
                            <div className="bg-white rounded-lg w-full max-w-md p-6 fade-in">
                                <div className="flex items-center justify-between mb-4">
                                    <h3 className="text-lg font-semibold">Share Something</h3>
                                    <button 
                                        onClick={() => setShowNewPost(false)}
                                        className="text-gray-400 hover:text-gray-600"
                                    >
                                        <X className="w-5 h-5" />
                                    </button>
                                </div>
                                
                                <div className="space-y-4">
                                    <div>
                                        <label className="block text-sm font-medium text-gray-700 mb-1">Type</label>
                                        <select 
                                            value={newPost.type}
                                            onChange={(e) => setNewPost(prev => ({ ...prev, type: e.target.value }))}
                                            className="w-full px-3 py-2 border border-gray-300 rounded-md"
                                        >
                                            <option value="story">Story</option>
                                            <option value="resource">Resource</option>
                                        </select>
                                    </div>
                                    
                                    <div>
                                        <label className="block text-sm font-medium text-gray-700 mb-1">Content</label>
                                        <textarea 
                                            value={newPost.content}
                                            onChange={(e) => setNewPost(prev => ({ ...prev, content: e.target.value }))}
                                            placeholder="Share your farming experience or resources..."
                                            className="w-full px-3 py-2 border border-gray-300 rounded-md h-24 resize-none"
                                        />
                                    </div>
                                    
                                    <div>
                                        <input 
                                            ref={fileInputRef}
                                            type="file" 
                                            accept="image/*,video/*"
                                            onChange={handleImageUpload}
                                            className="hidden"
                                        />
                                        <button 
                                            onClick={() => fileInputRef.current?.click()}
                                            className="flex items-center space-x-2 text-green-600 hover:text-green-700"
                                        >
                                            <Camera className="w-4 h-4" />
                                            <span>Add Photo/Video (Max 1MB)</span>
                                        </button>
                                        {newPost.image && (
                                            <img src={newPost.image} alt="Preview" className="mt-2 w-full h-32 object-cover rounded" />
                                        )}
                                    </div>
                                    
                                    <div className="flex space-x-3">
                                        <button 
                                            onClick={() => setShowNewPost(false)}
                                            className="flex-1 px-4 py-2 border border-gray-300 text-gray-700 rounded-md hover:bg-gray-50"
                                        >
                                            Cancel
                                        </button>
                                        <button 
                                            onClick={handleCreatePost}
                                            className="flex-1 px-4 py-2 bg-green-600 text-white rounded-md hover:bg-green-700"
                                        >
                                            Share
                                        </button>
                                    </div>
                                </div>
                            </div>
                        </div>
                    )}

                    <nav className="fixed bottom-0 left-0 right-0 bg-white border-t border-gray-200 shadow-lg">
                        <div className="flex justify-around py-2">
                            <button 
                                onClick={() => setActiveTab('home')}
                                className={`flex flex-col items-center py-2 px-3 ${activeTab === 'home' ? 'text-green-600' : 'text-gray-500'}`}
                            >
                                <Home className="w-5 h-5" />
                                <span className="text-xs mt-1">Home</span>
                            </button>
                            
                            <button 
                                onClick={() => setShowNewPost(true)}
                                className="flex flex-col items-center py-2 px-3 text-green-600"
                            >
                                <Plus className="w-5 h-5" />
                                <span className="text-xs mt-1">Share</span>
                            </button>
                            
                            <button 
                                onClick={() => setActiveTab('profile')}
                                className={`flex flex-col items-center py-2 px-3 ${activeTab === 'profile' ? 'text-green-600' : 'text-gray-500'}`}
                            >
                                <User className="w-5 h-5" />
                                <span className="text-xs mt-1">Profile</span>
                            </button>
                        </div>
                    </nav>
                </div>
            );
        };

        // Render the app
        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<FarmingApp />);
    </script>

    <div class="max-w-md mx-auto p-6 bg-white rounded-lg shadow-lg mt-8">
        <h2 class="text-xl font-bold text-green-700 mb-4">Next Steps for Deployment</h2>
        <p class="mb-4">Your app is now running in the browser. To deploy it to a live server:</p>
        
        <div class="mb-6">
            <h3 class="font-semibold text-green-600 mb-2">Option 1: Netlify (Recommended)</h3>
            <ol class="list-decimal pl-5 space-y-1">
                <li>Save this HTML file as <code class="bg-gray-100 px-1">index.html</code></li>
                <li>Create an account at <a href="https://netlify.com" class="text-blue-500">netlify.com</a></li>
                <li>Drag and drop the HTML file to deploy</li>
                <li>Your app will be live with a public URL</li>
            </ol>
        </div>
        
        <div class="mb-6">
            <h3 class="font-semibold text-green-600 mb-2">Option 2: GitHub Pages</h3>
            <ol class="list-decimal pl-5 space-y-1">
                <li>Save this HTML file as <code class="bg-gray-100 px-1">index.html</code></li>
                <li>Create a new GitHub repository</li>
                <li>Upload the file to the repository</li>
                <li>Go to Settings → Pages → Select main branch</li>
                <li>Your app will be live at <code class="bg-gray-100 px-1">username.github.io/repository-name</code></li>
            </ol>
        </div>
        
        <div>
            <h3 class="font-semibold text-green-600 mb-2">Option 3: Vercel</h3>
            <ol class="list-decimal pl-5 space-y-1">
                <li>Save this HTML file as <code class="bg-gray-100 px-1">index.html</code></li>
                <li>Create an account at <a href="https://vercel.com" class="text-blue-500">vercel.com</a></li>
                <li>Drag and drop the HTML file to deploy</li>
                <li>Your app will be live with a public URL</li>
            </ol>
        </div>
    </div>
</body>
</html>
