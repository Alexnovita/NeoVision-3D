git remote add origin https://github.com/Alexnovita/NeoVision3D.git
# NeoVision-3D
import { useState } from 'react';
import { motion } from 'framer-motion';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Textarea } from '@/components/ui/textarea';
import { Card, CardContent } from '@/components/ui/card';
import { ShoppingCart, Eye, Send, CheckCircle, XCircle, Facebook, Twitter, Instagram, Linkedin } from 'lucide-react';
import { Canvas } from '@react-three/fiber';
import { OrbitControls, Sphere, MeshDistortMaterial } from '@react-three/drei';
import emailjs from 'emailjs-com';

export default function NeoVision3D() {
  const [hover, setHover] = useState(false);
  const [form, setForm] = useState({ name: '', email: '', details: '' });
  const [message, setMessage] = useState(null);

  const handleChange = (e) => {
    setForm({ ...form, [e.target.name]: e.target.value });
  };

  const handleSubmit = async () => {
    if (form.name && form.email && form.details) {
      try {
        await emailjs.send(
          'service_neovision', // ID del servicio de EmailJS
          'template_cotizacion', // ID de la plantilla de EmailJS
          { name: form.name, email: form.email, details: form.details },
          'user_xxx123456789' // Tu Public API Key de EmailJS
        );
        setMessage({ type: 'success', text: 'Cotización enviada con éxito ✅' });
        setForm({ name: '', email: '', details: '' });
      } catch (error) {
        setMessage({ type: 'error', text: 'Error al enviar la cotización ❌' });
      }
    } else {
      setMessage({ type: 'error', text: 'Por favor llena todos los campos ❌' });
    }
  };

  const fireEffect = {
    scale: 1.2,
    boxShadow: '0 0 20px rgba(255, 165, 0, 0.6), 0 0 40px rgba(255, 69, 0, 0.8)',
    transition: { duration: 0.3, ease: 'ease-in-out' },
  };

  const socialLinks = [
    { href: 'https://facebook.com', icon: <Facebook className="mr-2" />, label: 'Facebook' },
    { href: 'https://twitter.com', icon: <Twitter className="mr-2" />, label: 'Twitter' },
    { href: 'https://instagram.com', icon: <Instagram className="mr-2" />, label: 'Instagram' },
    { href: 'https://linkedin.com', icon: <Linkedin className="mr-2" />, label: 'LinkedIn' },
  ];

  const faq = [
    { question: '¿Cómo puedo solicitar un modelo 3D?', answer: 'Puedes hacerlo a través del formulario de cotización o enviándonos un mensaje directo.' },
    { question: '¿Cuánto tiempo tardan en entregar un modelo?', answer: 'El tiempo de entrega depende de la complejidad del modelo, pero generalmente es entre 3 y 5 días hábiles.' },
    { question: '¿Ofrecen soporte post-venta?', answer: 'Sí, ofrecemos soporte para ajustes o modificaciones dentro de los 30 días posteriores a la entrega del modelo.' },
    { question: '¿Qué software utilizan para el modelado?', answer: 'Utilizamos Blender para crear los modelos 3D.' },
  ];

  return (
    <div className="min-h-screen bg-gradient-to-br from-black via-gray-900 to-black text-white overflow-hidden">
      <motion.header 
        initial={{ opacity: 0, y: -50 }} 
        animate={{ opacity: 1, y: 0 }} 
        transition={{ duration: 1 }}
        className="text-center py-10 text-4xl font-bold tracking-widest uppercase">
        NeoVision 3D
      </motion.header>

      <div className="grid grid-cols-1 md:grid-cols-3 gap-10 px-10">
        {[1, 2, 3].map((item) => (
          <Card 
            key={item} 
            className="bg-gray-800 rounded-2xl shadow-lg hover:shadow-xl transition-shadow duration-300 cursor-pointer"
            onMouseEnter={() => setHover(item)} 
            onMouseLeave={() => setHover(false)}>
            <CardContent className="p-6 text-center">
              <motion.div 
                animate={hover === item ? { scale: 1.1 } : { scale: 1 }} 
                transition={{ duration: 0.3 }}
                className="text-2xl mb-4">
                Modelo NeoVision #{item}
              </motion.div>
              <div className="w-full h-40 mb-4">
                <Canvas>
                  <OrbitControls enableZoom={false} />
                  <ambientLight intensity={0.5} />
                  <directionalLight position={[2, 2, 2]} />
                  <Sphere args={[1, 100, 200]} scale={1.2}>
                    <MeshDistortMaterial color={hover === item ? "#00ffcc" : "#ffffff"} distort={0.4} speed={2} />
                  </Sphere>
                </Canvas>
              </div>
              <div className="flex justify-center gap-4">
                <Button variant="outline">
                  <Eye className="mr-2" /> Ver Modelo
                </Button>
                <Button variant="default">
                  <ShoppingCart className="mr-2" /> Comprar
                </Button>
              </div>
            </CardContent>
          </Card>
        ))}
      </div>

      <div className="mt-20 px-10">
        <motion.div 
          initial={{ opacity: 0, y: 50 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 1 }}
          className="text-center text-3xl font-bold mb-10">
          Cotizaciones Personalizadas
        </motion.div>
        <div className="max-w-3xl mx-auto bg-gray-800 rounded-2xl p-10 shadow-lg">
          {message && (
            <motion.div 
              initial={{ opacity: 0 }}
              animate={{ opacity: 1 }}
              className={`mb-6 p-3 rounded-lg text-center ${message.type === 'success' ? 'bg-green-600' : 'bg-red-600'}`}>
              {message.type === 'success' ? <CheckCircle className="inline mr-2" /> : <XCircle className="inline mr-2" />}
              {message.text}
            </motion.div>
          )}
          <div className="mb-6">
            <Input name="name" value={form.name} onChange={handleChange} className="w-full p-3 rounded-lg" placeholder="Tu Nombre" />
          </div>
          <div className="mb-6">
            <Input name="email" value={form.email} onChange={handleChange} className="w-full p-3 rounded-lg" placeholder="Correo Electrónico" />
          </div>
          <div className="mb-6">
            <Textarea name="details" value={form.details} onChange={handleChange} className="w-full p-3 rounded-lg" placeholder="Detalles del Proyecto" rows={5} />
          </div>
          <Button variant="default" className="w-full" onClick={handleSubmit}>
            <Send className="mr-2" /> Enviar Cotización
          </Button>
        </div>
      </div>

      <div className="text-center mt-20">
        <a 
          href="/path/to/portafolio.pdf" 
          download 
          className="inline-block px-6 py-3 bg-gray-700 rounded-lg text-white">
          Descargar Portafolio
        </a>
      </div>

      <div className="mt-20 px-10 text-center">
        <motion.div 
          initial={{ opacity: 0, y: 50 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 1 }}
          className="text-3xl font-bold mb-10">
          Testimonios de Clientes
        </motion.div>
        <div className="flex flex-col items-center gap-8">
          <div className="bg-gray-800 p-6 rounded-lg shadow-lg max-w-3xl text-center">
            <p className="italic text-lg mb-4">"El trabajo de NeoVision fue increíble, el modelo 3D superó nuestras expectativas. ¡Altamente recomendado!"</p>
            <span className="font-bold">Carlos M., CEO de TechVision</span>
          </div>
          <div className="bg-gray-800 p-6 rounded-lg shadow-lg max-w-3xl text-center">
            <p className="italic text-lg mb-4">"Excelente calidad y puntualidad. Trabajar con ellos fue una experiencia increíble."</p>
            <span className="font-bold">Ana P., Diseñadora</span>
          </div>
        </div>
      </div>

      {/* FAQ Section */}
      <div className="mt-20 px-10 text-center">
        <motion.div 
          initial={{ opacity: 0, y: 50 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 1 }}
          className="text-3xl font-bold mb-10">
          Preguntas Frecuentes (FAQ)
        </motion.div>
        <div className="max-w-3xl mx-auto bg-gray-800 rounded-2xl p-10 shadow-lg">
          {faq.map((item, index) => (
            <div key={index} className="mb-6">
              <h3 className="text-lg font-semibold">{item.question}</h3>
              <p className="text-sm">{item.answer}</p>
            </div>
          ))}
        </div>
      </div>

      {/* Footer */}
      <div className="mt-20 px-10 text-center">
        <motion.div 
          initial={{ opacity: 0, y: 50 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 1 }}
          className="text-3xl font-bold mb-10">
          Síguenos en Redes Sociales
        </motion.div>
        <div className="flex justify-center gap-8">
          {socialLinks.map((link, index) => (
            <a key={index} href={link.href} target="_blank" rel="noopener noreferrer" className="text-white hover:text-yellow-400 transition-colors duration-300">
              {link.icon}
              <span>{link.label}</span>
            </a>
          ))}
        </div>
      </div>
    </div>
  );
}
