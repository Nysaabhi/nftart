# nftart
<!DOCTYPE html>
<html>
<head>
    <title>Luxury 360° NFT Gallery Experience</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
/* Root Variables */
:root {
    --primary-color: #8b5cf6;
    --accent-color: #c4b5fd;
    --background-dark: #1a1b26;
    --text-light: #e2e8f0;
    --gold: #ffd700;
    --marble: linear-gradient(45deg, #f3f4f6 0%, #e5e7eb 100%);
}

/* Base Reset */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    overflow: hidden;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: var(--background-dark);
    color: var(--text-light);
}

/* Loading Screen */
#loading-screen {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: var(--background-dark);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    z-index: 9999;
}

.loader {
    width: clamp(40px, 8vw, 60px);
    height: clamp(40px, 8vw, 60px);
    border: 5px solid var(--accent-color);
    border-top: 5px solid var(--gold);
    border-radius: 50%;
    animation: spin 1s linear infinite;
}

.loading-text {
    margin-top: 20px;
    font-size: clamp(1rem, 2vw, 1.2rem);
    color: var(--accent-color);
    letter-spacing: 2px;
}

@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}

/* Gallery Container */
#gallery {
    width: 100vw;
    height: 100vh;
    perspective: 1200px;
    position: relative;
}

.room {
    width: 100%;
    height: 100%;
    position: absolute;
    transform-style: preserve-3d;
    transition: transform 1.2s cubic-bezier(0.4, 0, 0.2, 1);
}

/* Wall Styles */
.wall {
    position: absolute;
    width: min(100vw, 1200px);
    height: min(100vh, 800px);
    background: linear-gradient(180deg, #2c3e50 0%, #1a1b26 100%);
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: clamp(10px, 2vw, 40px);
    padding: clamp(10px, 3vw, 40px);
    backdrop-filter: blur(10px);
    border: 2px solid rgba(255, 255, 255, 0.1);
    transform-origin: center;
}

.wall::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: url('/api/placeholder/1200/600') center/cover;
    opacity: 0.1;
    pointer-events: none;
}

/* Wall Positioning */
.front-wall { transform: translateZ(-50vw); }
.back-wall { transform: translateZ(50vw) rotateY(180deg); }
.left-wall { transform: translateX(-50vw) rotateY(90deg); }
.right-wall { transform: translateX(50vw) rotateY(-90deg); }

/* Floor and Ceiling */
.floor {
    position: absolute;
    width: min(100vw, 1200px);
    height: min(100vw, 1200px);
    background: var(--marble);
    transform: rotateX(90deg) translateZ(min(50vh, 400px));
    background-image: 
        linear-gradient(45deg, rgba(0,0,0,.1) 25%, transparent 25%),
        linear-gradient(-45deg, rgba(0,0,0,.1) 25%, transparent 25%),
        linear-gradient(45deg, transparent 75%, rgba(0,0,0,.1) 75%),
        linear-gradient(-45deg, transparent 75%, rgba(0,0,0,.1) 75%);
    background-size: 60px 60px;
    background-position: 0 0, 30px 0, 30px -30px, 0px 30px;
}

.ceiling {
    position: absolute;
    width: min(100vw, 1200px);
    height: min(100vw, 1200px);
    background: #1a1b26;
    transform: rotateX(-90deg) translateZ(min(50vh, 400px));
    box-shadow: inset 0 0 100px rgba(0,0,0,0.5);
}

/* Artwork Styles */
.artwork-frame {
    position: relative;
    width: 100%;
    aspect-ratio: 1;
    background: var(--marble);
    border-radius: clamp(8px, 2vw, 15px);
    padding: clamp(8px, 1.5vw, 15px);
    box-shadow: 
        0 10px 30px rgba(0,0,0,0.3),
        inset 0 0 15px rgba(0,0,0,0.2);
    transition: all 0.4s ease;
}

.artwork-frame::before {
    content: '';
    position: absolute;
    top: -5px;
    left: -5px;
    right: -5px;
    bottom: -5px;
    background: linear-gradient(45deg, var(--gold), var(--accent-color));
    z-index: -1;
    border-radius: clamp(10px, 2.5vw, 20px);
    opacity: 0.5;
    transition: opacity 0.4s ease;
}

.artwork-frame:hover::before {
    opacity: 1;
}

.artwork {
    width: 100%;
    height: 100%;
    background: rgba(255, 255, 255, 0.1);
    border-radius: clamp(5px, 1vw, 10px);
    overflow: hidden;
    cursor: pointer;
    transition: all 0.4s ease;
    position: relative;
}

.artwork:hover {
    transform: scale(1.02);
    box-shadow: 0 20px 30px rgba(0, 0, 0, 0.3);
}

.artwork img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.4s ease;
}

.artwork:hover img {
    transform: scale(1.1);
}

/* Price Tag */
.price-tag {
    position: absolute;
    top: clamp(10px, 2vw, 20px);
    right: clamp(10px, 2vw, 20px);
    background: rgba(0, 0, 0, 0.9);
    padding: clamp(5px, 1vw, 10px) clamp(10px, 2vw, 20px);
    border-radius: 20px;
    font-weight: 600;
    color: var(--gold);
    border: 2px solid var(--gold);
    backdrop-filter: blur(5px);
    font-size: clamp(12px, 1.5vw, 16px);
}

/* Artwork Info */
.artwork-info {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    background: rgba(0, 0, 0, 0.9);
    padding: clamp(10px, 2vw, 20px);
    transform: translateY(100%);
    transition: transform 0.3s ease;
    border-top: 2px solid var(--gold);
}

.artwork:hover .artwork-info {
    transform: translateY(0);
}

/* Controls */
.controls {
    position: fixed;
    bottom: clamp(10px, 3vw, 30px);
    left: 50%;
    transform: translateX(-50%);
    z-index: 1000;
    display: flex;
    gap: clamp(10px, 2vw, 20px);
    background: rgba(0, 0, 0, 0.8);
    padding: clamp(10px, 2vw, 20px) clamp(15px, 3vw, 30px);
    border-radius: 50px;
    backdrop-filter: blur(10px);
    border: 1px solid var(--gold);
}

.control-btn {
    padding: clamp(8px, 1.5vw, 15px) clamp(16px, 3vw, 30px);
    background: var(--primary-color);
    color: white;
    border: none;
    border-radius: 25px;
    cursor: pointer;
    font-weight: 600;
    transition: all 0.3s ease;
    display: flex;
    align-items: center;
    gap: 10px;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-size: clamp(12px, 1.5vw, 16px);
}

.control-btn:hover {
    background: var(--accent-color);
    transform: translateY(-2px);
    box-shadow: 0 5px 15px rgba(0,0,0,0.3);
}

/* Modal */
.modal {
    display: none;
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: rgba(26, 27, 38, 0.98);
    padding: clamp(20px, 4vw, 40px);
    border-radius: clamp(15px, 3vw, 30px);
    z-index: 2000;
    width: min(90%, 700px);
    backdrop-filter: blur(20px);
    border: 2px solid var(--gold);
    box-shadow: 0 25px 50px rgba(0,0,0,0.5);
}

.modal-content {
    text-align: center;
}

.modal img {
    max-width: 100%;
    border-radius: clamp(10px, 2vw, 20px);
    margin-bottom: clamp(15px, 3vw, 30px);
    border: 3px solid var(--gold);
}

.modal h2 {
    color: var(--gold);
    margin-bottom: 15px;
    font-size: clamp(1.5rem, 3vw, 2rem);
}

.buy-btn {
    background: linear-gradient(45deg, var(--primary-color), var(--accent-color));
    color: white;
    padding: clamp(12px, 2vw, 18px) clamp(24px, 4vw, 36px);
    border: none;
    border-radius: 30px;
    cursor: pointer;
    font-weight: 600;
    margin-top: 20px;
    transition: all 0.3s ease;
    text-transform: uppercase;
    letter-spacing: 2px;
    font-size: clamp(14px, 1.5vw, 16px);
}

.buy-btn:hover {
    transform: translateY(-3px);
    box-shadow: 0 10px 20px rgba(0,0,0,0.3);
}

/* Navigation Hints */
#navigation-hints {
    position: fixed;
    top: clamp(10px, 2vw, 20px);
    left: 50%;
    transform: translateX(-50%);
    background: rgba(0, 0, 0, 0.8);
    padding: clamp(8px, 1.5vw, 15px) clamp(15px, 2.5vw, 25px);
    border-radius: 25px;
    display: flex;
    gap: clamp(10px, 2vw, 20px);
    align-items: center;
    backdrop-filter: blur(10px);
    border: 1px solid var(--gold);
    z-index: 100;
}

.hint {
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: clamp(12px, 1.4vw, 14px);
    color: var(--text-light);
}

.key {
    background: rgba(255, 255, 255, 0.2);
    padding: clamp(4px, 1vw, 8px) clamp(6px, 1.5vw, 12px);
    border-radius: 8px;
    font-weight: 600;
    border: 1px solid var(--accent-color);
}

/* Media Queries */
@media (max-width: 768px) {
    .wall {
        grid-template-columns: repeat(2, 1fr);
        width: 90vw;
        height: 90vh;
    }

    .front-wall { transform: translateZ(-45vw); }
    .back-wall { transform: translateZ(45vw) rotateY(180deg); }
    .left-wall { transform: translateX(-45vw) rotateY(90deg); }
    .right-wall { transform: translateX(45vw) rotateY(-90deg); }
}

@media (max-width: 480px) {
    .wall {
        grid-template-columns: 1fr;
        width: 80vw;
        height: 80vh;
        gap: 15px;
        padding: 15px;
    }

    .front-wall { transform: translateZ(-40vw); }
    .back-wall { transform: translateZ(40vw) rotateY(180deg); }
    .left-wall { transform: translateX(-40vw) rotateY(90deg); }
    .right-wall { transform: translateX(40vw) rotateY(-90deg); }

    #navigation-hints {
        display: none;
    }

    .controls {
        padding: 8px 12px;
    }

    .control-btn {
        padding: 6px 12px;
        font-size: 12px;
    }
}

@media (max-height: 480px) and (orientation: landscape) {
    .wall {
        grid-template-columns: repeat(4, 1fr);
        height: 70vh;
    }

    .artwork-frame {
        aspect-ratio: 16/9;
    }

    .controls {
        bottom: 5px;
    }
}
    </style>
</head>
<body>
    <div id="loading-screen">
        <div class="loader"></div>
        <div class="loading-text">ENTERING THE GALLERY...</div>
    </div>

    <div id="navigation-hints">
        <div class="hint">
            <span class="key">←</span>
            <span>Rotate Left</span>
        </div>
        <div class="hint">
            <span class="key">→</span>
            <span>Rotate Right</span>
        </div>
        <div class="hint">
            <span class="key">ESC</span>
            <span>Close Preview</span>
        </div>
    </div>

    <div id="gallery">
        <div class="room">
            <div class="wall front-wall">
                <div class="artwork-frame">
                    <div class="artwork" data-id="1">
                        <div class="price-tag">2.5 ETH</div>
                        <img src="https://i.ibb.co/yFNbRqF/Whats-App-Image-2024-10-20-at-09-16-07-510389dd.jpg" alt="NFT 1">
                        <div class="artwork-info">
                            <h3>Digital Dreams #1</h3>
                            <p>A mesmerizing digital artwork</p>
                        </div>
                    </div>
                </div>
                <div class="artwork-frame">
                    <div class="artwork" data-id="2">
                        <div class="price-tag">3.8 ETH</div>
                        <img src="https://i.ibb.co/7vkRDZv/Whats-App-Image-2024-10-20-at-09-16-08-0798f91f.jpg" alt="NFT 2">
                        <div class="artwork-info">
                            <h3>Cosmic Voyage #4</h3>
                            <p>Journey through digital space</p>
                        </div>
                    </div>
                </div>
                <div class="artwork-frame">
                    <div class="artwork" data-id="3">
                        <div class="price-tag">5.2 ETH</div>
                        <img src="https://i.ibb.co/zJtnbYT/Whats-App-Image-2024-09-14-at-20-56-26-65ff10ed.jpg" alt="NFT 3">
                        <div class="artwork-info">
                            <h3>Neural Nexus</h3>
                            <p>AI-generated masterpiece</p>
                        </div>
                    </div>
                </div>
            </div>
            <div class="wall back-wall">
                <div class="artwork-frame">
                    <div class="artwork" data-id="4">
                        <div class="price-tag">4.1 ETH</div>
                        <img src="https://i.ibb.co/34hNCYB/a-painting-of-a-woman-s-face-with-many-different-c-k-NYVSe-CYTYiq4u-Ra-Tf-b-XA-j-Uy-R1w-CRum-D8rj-Pa.jpg" alt="NFT 4">
                        <div class="artwork-info">
                            <h3>Quantum Realms</h3>
                            <p>Abstract digital dimensions</p>
                        </div>
                    </div>
                </div>
                <div class="artwork-frame">
                    <div class="artwork" data-id="5">
                        <div class="price-tag">6.7 ETH</div>
                        <img src="https://i.ibb.co/JB62R8k/Whats-App-Image-2024-09-14-at-20-56-23-1fbc6812.jpg" alt="NFT 5">
                        <div class="artwork-info">
                            <h3>Cyber Genesis</h3>
                            <p>Birth of digital consciousness</p>
                        </div>
                    </div>
                </div>
                <div class="artwork-frame">
                    <div class="artwork" data-id="6">
                        <div class="price-tag">3.3 ETH</div>
                        <img src="https://i.ibb.co/8Y2R6M6/Whats-App-Image-2024-08-29-at-01-47-07-f81411a2.jpg" alt="NFT 6">
                        <div class="artwork-info">
                            <h3>Virtual Visions</h3>
                            <p>Exploring digital dreams</p>
                        </div>
                    </div>
                </div>
            </div>
            <div class="wall left-wall">
                <div class="artwork-frame">
                    <div class="artwork" data-id="7">
                        <div class="price-tag">4.5 ETH</div>
                        <img src="/api/placeholder/400/400" alt="NFT 7">
                        <div class="artwork-info">
                            <h3>Digital Dawn</h3>
                            <p>First light in the metaverse</p>
                        </div>
                    </div>
                </div>
                <div class="artwork-frame">
                    <div class="artwork" data-id="8">
                        <div class="price-tag">5.9 ETH</div>
                        <img src="/api/placeholder/400/400" alt="NFT 8">
                        <div class="artwork-info">
                            <h3>Ethereal Echo</h3>
                            <p>Resonating through dimensions</p>
                        </div>
                    </div>
                </div>
                <div class="artwork-frame">
                    <div class="artwork" data-id="9">
                        <div class="price-tag">7.2 ETH</div>
                        <img src="/api/placeholder/400/400" alt="NFT 9">
                        <div class="artwork-info">
                            <h3>Meta Masterpiece</h3>
                            <p>Bridging reality and virtuality</p>
                        </div>
                    </div>
                </div>
            </div>
            <div class="wall right-wall">
                <div class="artwork-frame">
                    <div class="artwork" data-id="10">
                        <div class="price-tag">4.8 ETH</div>
                        <img src="/api/placeholder/400/400" alt="NFT 10">
                        <div class="artwork-info">
                            <h3>Pixel Paradise</h3>
                            <p>Digital utopia unleashed</p>
                        </div>
                    </div>
                </div>
                <div class="artwork-frame">
                    <div class="artwork" data-id="11">
                        <div class="price-tag">6.3 ETH</div>
                        <img src="/api/placeholder/400/400" alt="NFT 11">
                        <div class="artwork-info">
                            <h3>Crypto Canvas</h3>
                            <p>Blockchain brushstrokes</p>
                        </div>
                    </div>
                </div>
                <div class="artwork-frame">
                    <div class="artwork" data-id="12">
                        <div class="price-tag">5.5 ETH</div>
                        <img src="/api/placeholder/400/400" alt="NFT 12">
                        <div class="artwork-info">
                            <h3>Digital Dynasty</h3>
                            <p>Legacy of the future</p>
                        </div>
                    </div>
                </div>
            </div>
            <div class="floor"></div>
            <div class="ceiling"></div>
        </div>
    </div>

    <div class="controls">
        <button class="control-btn" onclick="rotate('left')">
            <i class="fas fa-chevron-left"></i> Rotate Left
        </button>
        <button class="control-btn" onclick="rotate('right')">
            Rotate Right <i class="fas fa-chevron-right"></i>
        </button>
    </div>

    <div class="modal" id="nftModal">
        <span class="close-modal">×</span>
        <div class="modal-content">
            <img src="" alt="NFT Preview">
            <h2></h2>
            <p class="artist-info"></p>
            <p class="description"></p>
            <p class="price"></p>
            <button class="buy-btn">
                <i class="fas fa-shopping-cart"></i> Buy Now
            </button>
        </div>
    </div>

    <script>
        // Enhanced NFT data with more details
        const nftData = {
            1: { 
                title: "Digital Dreams #1",
                artist: "CryptoArtist",
                description: "A mesmerizing digital artwork that explores the boundaries between reality and imagination. Created using advanced AI algorithms and traditional digital painting techniques.",
                price: "2.5 ETH",
                created: "2024-03-15",
                edition: "1 of 1",
                blockchain: "Ethereum",
                collection: "Digital Dreams Series"
            },
            2: {
                title: "Cosmic Voyage #4",
                artist: "NeuroArtisan",
                description: "An interstellar journey through digital realms, representing the infinite possibilities of space and technology.",
                price: "3.8 ETH",
                created: "2024-03-18",
                edition: "1 of 3",
                blockchain: "Ethereum",
                collection: "Cosmic Series"
            },
            // Additional NFT data entries follow the same pattern...
        };

        let currentRotation = 0;
        const room = document.querySelector('.room');
        const modal = document.getElementById('nftModal');

        // Hide loading screen after content loads with a smooth fade
        window.addEventListener('load', () => {
            setTimeout(() => {
                const loadingScreen = document.getElementById('loading-screen');
                loadingScreen.style.transition = 'opacity 0.5s ease';
                loadingScreen.style.opacity = '0';
                setTimeout(() => {
                    loadingScreen.style.display = 'none';
                }, 500);
            }, 1500);
        });

        function rotate(direction) {
            if (direction === 'left') {
                currentRotation -= 90;
            } else {
                currentRotation += 90;
            }
            room.style.transform = `rotateY(${currentRotation}deg)`;
        }

        document.querySelectorAll('.artwork').forEach(artwork => {
            artwork.addEventListener('click', function() {
                const id = this.dataset.id;
                const nft = nftData[id];
                
                const modalContent = modal.querySelector('.modal-content');
                modalContent.querySelector('img').src = this.querySelector('img').src;
                modalContent.querySelector('h2').textContent = nft.title;
                modalContent.querySelector('.artist-info').textContent = `By ${nft.artist} • Created ${nft.created}`;
                modalContent.querySelector('.description').textContent = nft.description;
                modalContent.querySelector('.price').innerHTML = `
                    <strong style="color: var(--gold)">Price:</strong> ${nft.price}<br>
                    <span style="font-size: 0.9em; color: var(--accent-color)">
                        ${nft.edition} • ${nft.blockchain} • ${nft.collection}
                    </span>
                `;
                
                modal.style.display = 'block';
                modal.style.opacity = '0';
                requestAnimationFrame(() => {
                    modal.style.transition = 'opacity 0.3s ease';
                    modal.style.opacity = '1';
                });
            });
        });

        function closeModal() {
            modal.style.opacity = '0';
            setTimeout(() => {
                modal.style.display = 'none';
            }, 300);
        }

        document.querySelector('.close-modal').addEventListener('click', closeModal);

        function buyNFT() {
            const btn = document.querySelector('.buy-btn');
            btn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> Connecting Wallet...';
            btn.style.opacity = '0.7';
            btn.style.cursor = 'not-allowed';
            
            setTimeout(() => {
                alert('Connecting to NFT marketplace...');
                btn.innerHTML = '<i class="fas fa-shopping-cart"></i> Buy Now';
                btn.style.opacity = '1';
                btn.style.cursor = 'pointer';
            }, 1500);
        }

        document.querySelector('.buy-btn').addEventListener('click', buyNFT);

        window.onclick = function(event) {
            if (event.target == modal) {
                closeModal();
            }
        }

        // Enhanced keyboard controls
        document.addEventListener('keydown', function(e) {
            if (e.key === 'ArrowLeft') {
                rotate('left');
                document.querySelector('.control-btn:first-child').classList.add('active');
                setTimeout(() => document.querySelector('.control-btn:first-child').classList.remove('active'), 200);
            } else if (e.key === 'ArrowRight') {
                rotate('right');
                document.querySelector('.control-btn:last-child').classList.add('active');
                setTimeout(() => document.querySelector('.control-btn:last-child').classList.remove('active'), 200);
            } else if (e.key === 'Escape') {
                closeModal();
            }
        });

        // Enhanced touch support for mobile devices
        let touchStartX = 0;
        let touchEndX = 0;

        document.addEventListener('touchstart', e => {
            touchStartX = e.touches[0].clientX;
        });

        document.addEventListener('touchmove', e => {
            touchEndX = e.touches[0].clientX;
        });

        document.addEventListener('touchend', () => {
            const swipeDistance = touchEndX - touchStartX;
            if (Math.abs(swipeDistance) > 50) {
                if (swipeDistance > 0) {
                    rotate('left');
                } else {
                    rotate('right');
                }
            }
        });
    </script>
</body>
</html>
